---
path: '/2008/08/shell-commands-to-upgrade-to-wp-261/'
date: '2008-08-29'
title: 'Shell commands to upgrade to WordPress 2.6.1'
---

Just upgraded this blog to WordPress 2.6.1 via ssh. Went just fine (knock on wood). Here’s the commands I used, for my own reference as much as anyone else’s:

```
wget http://wordpress.org/latest.tar.gz
rm -rf wp-includes
rm -rf wp-admin
tar -xzvf latest.tar.gz
cp -rpf wordpress/\* .
rm latest.tar.gz
```

Of course, you’ll want to replace “.” in the second-to-last line with the address of your wordpress directory. In my case, I use the root directory.

UPDATE: Another tip I thought of to make this process even more streamlined: paste all of the above commands into a text file and call it “wp-upgrade” or something like that. Then type in:
`chmod 755 wp-upgrade`
That will make the file executable. From then on, all you have to do to upgrade WordPress is type `./wp-upgrade.`
