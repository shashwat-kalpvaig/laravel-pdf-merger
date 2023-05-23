# Laravel PDF Merger

PDF merger for Laravel inspired by another package, created for personal use. Tested with Laravel 5.6.

## Advantages
* Also works with PDF versions above `1.4`
* Works with `PHP 7`

## Installation
```bash
 $ composer require kalpvaig/laravel-pdf-merger
```

## Configuration
Make the following changes to the main configuration file located at `config/app.php`
```php
'providers' => [
   ...
   Kalpvaig\LaravelPDFMerger\Providers\PDFMergerServiceProvider::class
],

'aliases' => [
   ...
   'PDFMerger' => Kalpvaig\LaravelPDFMerger\Facades\PDFMergerFacade::class
]
```

> When merging PDFs versions above 1.4 or PDF strings, a temporary PDF will be created during the process and stored in `storage/app/public/tmp` directory, therefore you may need to create it beforehand.
> Also, note that this package requires Ghostscript installed on the server in order to functiona properly with PDF versions 1.5+. [Install Guide](https://www.ghostscript.com/doc/9.20/Install.htm)


## Required Packages for Ubuntu Server
```Install the following package
- `sudo apt install ghostscript`
// ghostscript should be installed on your machine
```


## Usage

You can add PDFs for merging, by specifying a file path of PDF with `addPathToPDF` method, or adding PDF file as string with `addPDFString` method. The second argument of both methods is array of selected pages (`'all'` for all pages) and the third argument is PDFs orientation (portrait or landscape).
```php
$merger->addPathToPDF('/path/to/pdf', 'all', 'P');
$merger->addPDFString(file_get_contents('path/to/pdf'), ['1', '2'], 'L')
```

You can set a merged PDF name by using `setFileName` method.
```php
$merger->setFileName('merger.pdf');
```

In the end, finnish process with `merge` or `duplexMerge` method and use one of the output options for the merged PDF. The difference bwetween two methods is, that `duplexMerge` adds blank page after each merged PDF, if it has odd number of pages.

Available output options are:
  * `inline()`
  * `download()`
  * `string()`
  * `save('path/to/merged.pdf')`

```php
$merger->merge();
$merger->inline();
```

## Example usage
```php
$merger = new PDFMerger(new Filesystem());

$merger->addPathToPDF(base_path('/vendor/kalpvaig/laravel-pdf-merger/examples/one.pdf'), [2], 'P');
$merger->addPDFString(file_get_contents(base_path('/vendor/kalpvaig/laravel-pdf-merger/examples/two.pdf')), 'all', 'L');

// Or You can use A for detecting automatic orientation
$merger->addPDFString(file_get_contents(base_path('/vendor/kalpvaig/laravel-pdf-merger/examples/three.pdf')), 'all', 'A');


$merger->merge();
$merger->save(base_path('/public/pdfs/merged.pdf'));
```

## Authors
* [GrofGraf](https://github.com/GrofGraf)
* [Shashwat] (https://github.com/50shashwat)


## Credits
* **Webklex** [LaravelPDFMerger](https://github.com/Webklex/laravel-pdfmerger)

## License
The MIT License (MIT)

Copyright (c) 2017 Shashwat (Previous Maintainer - GrofGraf)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
