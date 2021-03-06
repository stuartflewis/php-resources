---
title: "How to Manage Wildcard Subdomains in PHP?"
updated: "August 29, 2015"
permalink: "/faq/wildcart-subdomains/"
---

Wildcard subdomains are used for user specific sites, Deviantart, blogspot.com
are good examples. Usually DNS handles the subdomains depending on A and CNAME
records.

Using `.htaccess` will give control on incoming trafic (URL translation).
Below `.htaccess` code will redirect any subdomain to PHP page.

```
RewriteCond %{HTTP_HOST} !^www\.domain\.com
RewriteCond %{HTTP_HOST} ([^.]+)\.domain\.com [NC]
RewriteRule ^/?$ /member.php?username=%1 [L]
```

`.htacces` code will forward incoming request to `member.php` with GET method.
So anyone who types `joy.domain.com` will get content of
`domain.com/member.php?username=joy`. By using SQL query you can verify if the
username exists or not.

```php?start_inline=1
$sth = $dbh->prepare("SELECT * FROM user WHERE username=:username");
$sth->bindParam(':username', $username, PDO::PARAM_STR);
$sth->execute();
$result = $sth->fetch(PDO::FETCH_ASSOC);
if ($result) {
    // show user data
} else {
    // show 404 page
}
```
