---
path: '/2009/04/how-to-sort-by-date-with-nutch/'
date: '2009-04-19'
title: 'How to sort by date with Nutch'
---

I’ve been working with a team over the last few months to create a search engine that could sort by date for the local student newspaper. Among the open-source search engines available, Nutch seems to be the easiest to set up. However, I couldn’t find any tutorials on sorting by date so I decided to write this one.

Nutch, in its default configuration, will sort pages by relevance using Lucene scores. You can get Nutch to sort by another field in its index using the sort parameter, but first, you need a field to sort by. (For an example of how sorting works, add &sort=url to the end of your Nutch query. Obviously sorting by url isn’t very useful, but you can see how the query works. Note that you can also add &reverse=true to sort the results in reverse order.)

Sorting by date in Nutch essentially involves two parts: first, using a plugin to get Nutch to index dates into a new field in your index, and second, getting your query page to add the additional parameter to search queries.

Indexing dates with a Nutch plugin

NOTE: I figured out most of the information here from the tutorial on writing a Nutch plugin available on the Nutch Wiki. If you need more detailed information on any of the steps here I recommend reading that tutorial.

All Nutch plugins implement an interface. These interfaces are called extension points. The Nutch Wiki has a list of available extension points. The goal of our plugin is to add an additional date field to the Lucene index, so we want to implement the IndexingFilter interface.

Nutch plugins consist of xml description files and your Java source code files. File structure is as follows:

```
/nutch-dir
  /src
    /plugin
      /yourpluginname
        build.xml
        plugin.xml
        /src
          /org
            /apache
              /nutch
                /yourpluginname
                  ImplementationClassName.java
```

Here’s an example build.xml file:

```
<?xml version="1.0"?>
<project name="yourpluginname" default="jar">
  <import file="../build-plugin.xml"/>
</project>
```

And here’s an example plugin.xml file:

```
<?xml version="1.0" encoding="UTF-8"?>
<plugin
   id="yourpluginname"
   name="longer/human-readable name of your plugin"
   version="0.0.1"
   provider-name="your_website">

   <runtime>
      <library name="yourpluginname.jar">
         <export name="*"/>
      </library>
   </runtime>

   <extension id="org.apache.nutch.urldateindexer.yourpluginname"
              name="longer/human-readable name of your plugin"
              point="org.apache.nutch.indexer.IndexingFilter">
      <implementation id="ImplementationClassName"
                      class="org.apache.nutch.yourpluginname.ImplementationClassName"/>
   </extension>
</plugin>
```

NOTE: I had some trouble using my own package name, but it might have been due to another problem. When in doubt I recommend sticking with org.apache.nutch.yourpluginname.

And here’s the code for the Java source file, ImplementationClassName.java. (You’ll want to change it to your own name to match what you put in the plugin.xml file.)

```
package org.apache.nutch.yourpluginname;

//imports
import java.util.Date;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.io.Text;
import org.apache.lucene.document.DateTools;
import org.apache.lucene.document.Document;
import org.apache.lucene.document.Field;
import org.apache.lucene.document.DateTools.Resolution;
import org.apache.nutch.crawl.CrawlDatum;
import org.apache.nutch.crawl.Inlinks;
import org.apache.nutch.indexer.IndexingException;
import org.apache.nutch.indexer.IndexingFilter;
import org.apache.nutch.parse.Parse;

public class UrlDateIndexer implements IndexingFilter {

  private Configuration conf;

  public UrlDateIndexer() {
  }

  //this is where your code to add a new field to the index goes
  public Document filter(Document doc, Parse parse, Text url,
    CrawlDatum datum, Inlinks inlinks)
    throws IndexingException {

    // Get url from the method inputs
    String urlString = url.toString();

    // Use regex to identify type of date and parse date from url format (code for the TdcDateParser class not shown)
    TdcDateParser tdcDateParser = new TdcDateParser();
    Date publishedDate = tdcDateParser.parseUrl(urlString);

    // Store date in field
    if (null != publishedDate){
    	//create a new field
      Field publishedDateField = new Field("published_date",DateTools.dateToString(publishedDate, Resolution.DAY),
           Field.Store.YES, Field.Index.UN_TOKENIZED);

      //add the field to the index
    	doc.add(publishedDateField);
    }

    return doc;
  }

  public void setConf(Configuration conf) {
    this.conf = conf;
  }

  public Configuration getConf() {
    return this.conf;
  }
}
```

In the case of the plugin I build, the goal was to parse the date from the url of a page. You’ll need to modify this method to get the date from wherever is appropriate for your search engine. The basic flow is to create a field, add that field to a document and then return the document.

There are a few things to note about the line that creates the new field:

```
Field publishedDateField = new Field("published_date",DateTools.dateToString(publishedDate, Resolution.DAY),
    Field.Store.YES, Field.Index.UN_TOKENIZED);
```

dateToString: More recent versions of Lucene don’t store dates in the index directly. Therefore, you need to use the dateToString method to store dates.
Field.Store.YES: This stores the text of the field in the index. You wouldn’t do this for a long field that you don’t need to view in its entirety in the search engine (e.g. page content). I believe setting Field.Store to YES is mandatory to be able to sort by a field.
Field.Index.UN_TOKENIZED: Tells Lucene not to break up the field into individual words, as it would with a page’s content. I also believe this is mandatory to be able to sort by a field.
Now that you have finished creating your files, upload them to the nutch directory on your server if you haven’t already done so. Then, add a line like the following to the build.xml file in the /nutch-dir/src/plugin directory:

```
  <ant dir="yourpluginname" target="deploy"/>
```

Then launch ant from the command line in that same directory (/nutch-dir/src/plugin). This should compile the classes and jar file for your plugin and place them in the /nutch-dir/build directory. It should also place a copy of the jar file and plugin.xml file in the /nutch-dir/plugins directory.

NOTE: I had to modify the deploy.dir property in the /nutch-dir/src/plugin/build-plugin.xml file to get ant to put the jar and plugin.xml file in the right plugins directory. Originally it was set to put them in build/plugins. I’m not sure which directory is formally correct, but the /nutch-dir/plugins directory is where Nutch was set to read from on my system. If this is the case for you, just remove /build from the deploy.dir line. My resulting deploy.dir line is as follows:

```
<property name="deploy.dir" location="${nutch.root}/plugins/${name}"/>
```

Next, you need to modify the /nutch-dir/conf/nutch-site.xml file to recognize your new plugin. Just add your plugin name to the plugin.includes property. For example:

```
<property>
  <name>plugin.includes</name>
  <value>yourpluginname|protocol-http|urlfilter-regex|parse-(text|html|js)|index
-basic|query-(basic|site|url)|summary-basic|scoring-opic|urlnormalizer-(pass|reg
ex|basic)</value>
</property>
```

Launch a Nutch crawl of just a few pages to test your new plugin. To see if the plugin was activated by Nutch, open /nutch-dir/logs/hadoop.log and look for a line like the following:

```
2009-04-17 10:54:27,477 INFO  plugin.PluginRepository - Indexer of dates from URLs (urldateindexer)
If the plugin didn’t show up, make sure your plugin jar and xml files are in the directory indicated in these log file lines:

2009-04-17 10:54:23,617 INFO  plugin.PluginRepository - Plugins: looking in: /nutch-0.9/plugins
2009-04-17 10:54:24,062 INFO  plugin.PluginRepository - Plugin Auto-activation mode: [true]
2009-04-17 10:54:24,062 INFO  plugin.PluginRepository - Registered Plugins:
```

Finally, test to see whether your plugin actually added fields to the index. You can do this using Luke. Download the yourcrawl/index directory from your crawl to your local machine and open it with Luke. Check that the name of your new field is listed in the fields list and that the field holds the expected values for your sample documents. If not, modify your source code, run ant in the /nutch-dir/src/plugin directory to rebuild the jar file and run your crawl again.

Sorting by date from your query page

You’re almost done! Now that your new field is in the index, you just need to modify your query page to sort by the new field. The default nutch query pages are /apache-tomcat-6.0.14/webapps/ROOT/search.jsp and (language)/search.html in that directory.

If you always want to sort by date, just find the line of code with input name="query" in it and add a parameter to sort by date after it. For example:

```
<input name="query" size="44" /> <input type="submit" value="Search" />
<input type="hidden" name="sort" value="published_date" />
```

Note that you may also want to add the following parameter to make Nutch put the most recent dates first.

```
<input type="hidden" name="reverse" value="true" />
```

If you want to do something more sophisticated, like only sort by date in certain situations, you can modify the code in search.jsp. Look for the following line, and modify it as needed:

String sort = request.getParameter("sort");
For instance, I added a checkbox to my site that let users chose whether to sort by date or not. Since this checkbox returned a value only if it was checked, I wrote code that used the presence of that parameter to determine whether to assign a value to sort.

```
  if (request.getParameter("sort_by_date") != null){
    sort = "published_date";
    reverse = true;
  }
```

Good luck with your own Nutch sorting projects! Feel free to leave a comment or contact me if you have any questions.
