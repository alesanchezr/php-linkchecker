# php-linkchecker 

Check broken links in html / json files, sitemap.xml and robots.txt.

It's working with :

*   [Guzzle](http://docs.guzzlephp.org)
*   [Symfony Finder Component](http://symfony.com/doc/2.3/components/finder.html)
*   [Glicer Simply-html Component](https://github.com/emmanuelroecker/php-simply-html)

## Installation

This library can be found on [Packagist](https://packagist.org/packages/glicer/link-checker).

The recommended way to install is through [composer](http://getcomposer.org).

Edit your `composer.json` and add:

```json
{
    "require": {
       "glicer/link-checker": "dev-master"
    }
}
```

And install dependencies:

```bash
php composer.phar install
```

## How to check links in html / json files ?

```php
<?php
    require 'vendor/autoload.php';

    use GlLinkChecker\GlLinkChecker;
    use Symfony\Component\Finder\Finder;

    //relative url use host http://lyon.glicer.com to check link
    $linkChecker  = new GlLinkChecker('http://lyon.glicer.com');

    //construct list of local html and json files to check
    $finder = new Finder();
    $files  = $finder->files()->in('./public')->name("*.html")->name("*.json");

    //launch links checking
    $result  = $linkChecker->checkFiles(
        $files,
        function ($nbr) {
            // called at beginning - $nbr urls to check
        },
        function ($url, $files) {
            // called each $url - $files : list of filename containing $url link
        },
        function () {
            // called at the end
        }
    );

    //convert $result array in a temp html file
    $filereport = GlLinkCheckerReport::toTmpHtml('lyonCheck',$result);

    //$filereport contain fullpath to html file
    print_r($filereport);
```

you can view $filereport with your browser

## How to check links in robots.txt and sitemap files ?

```php
<?php
     require 'vendor/autoload.php';

     $linkChecker = new GlLinkChecker('http://lyon.glicer.com');
     $result      = $linkChecker->checkRobotsSitemap();

     print_r($result);
```

GlLinkChecker::checkRobotsSitemap() return an array like :

```php
    $result = [
        'disallow' =>
            ['error' => ['/img/', '/download/']],
        'sitemap'  =>
            [
                'ok' => [
                    '/sitemap.xml' =>
                        [
                            'ok' =>
                                [
                                    '/index.html',
                                    '/section/probleme-solution/compresser-css-html-js.html'
                                ]
                        ]
                ]
            ]
    ];
```

## Contact

Authors : Emmanuel ROECKER & Rym BOUCHAGOUR

[Web Development Blog - http://dev.glicer.com](http://dev.glicer.com)