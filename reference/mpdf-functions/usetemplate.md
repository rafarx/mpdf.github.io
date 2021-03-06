---
layout: page
title: UseTemplate() (until 8.0)
parent_title: mPDF functions
permalink: /reference/mpdf-functions/usetemplate.html
modification_time: 2019-03-25T08:21:00+00:00
---

(mPDF &ge; 2.3 && mPDF < 8.0)

UseTemplate – Insert an imported page from an external PDF file

<div class="alert alert-info" role="alert" markdown="1">
  **Note:** This method was superseded by <a href="{{ "/reference/mpdf-functions/usetemplate-v8.html" | prepend: site.baseurl }}">useTemplate()</a> in mPDF 8.0.
</div>

# Description

array **UseTemplate** (
int <span class="parameter">$templateID</span>
[, float <span class="parameter">$x</span>
[, float <span class="parameter">$y</span>
[, float <span class="parameter">$width</span>
[, float <span class="parameter">$height</span>
]]]])

Insert an imported page/template from an external PDF file into the current document. The page, or 'cropped' page, must have
already been stored as a 'template' using <a href="{{ "/reference/mpdf-functions/setsourcefile.html" | prepend: site.baseurl }}">SetSourceFile()</a>.
The template is inserted on the current page of the document. `UseTemplate()` returns an array of height and width of the
imported page as it is printed (see Example #1).

<div class="alert alert-info" role="alert" markdown="1">
  **Note:** The template will be printed onto the page as the bottom 'layer' i.e.
	anything else written to that page by mPDF will be written on top of the template. NB If you use `WriteHTML()` and have
	a background-color set on BODY this will hide the template from view e.g.
  `<body style="background-color:#FFFFFF;">`
</div>

<div class="alert alert-info" role="alert" markdown="1">
  **Note:** If you are using automatic header-margins, you need to set the header before starting the first page; if you start
  the document with `UseTemplate()` this will move it to page 1, so the order needs to be:

  ```php
  <?php
  $mpdf = new \Mpdf\Mpdf();
  $mpdf->SetImportUse();

  $mpdf->SetHTMLHeader($header);

  $pagecount = $mpdf->SetSourceFile('logoheader.pdf');
  $tplIdx = $mpdf->ImportPage($pagecount);
  $mpdf->UseTemplate($tplIdx);

  $mpdf->WriteHTML($html);
  ```
</div>

## Parameters

<span class="parameter">$templateID</span>

: This parameter specifies the ID of the page template to insert.

<span class="parameter">$x</span>

: Sets the <span class="parameter">$x</span> co-ordinate (abscissa) to output the template. Value should be
  specified as <span class="smallblock">LENGTH</span> in millimetres.

  Default: `null` - Sets <span class="parameter">$x</span> co-ordinate to `0`

  <span class="smallblock">BLANK</span> or omitted - uses default (`0`)

  `-1`: Uses current writing position in document

<span class="parameter">$y</span>

: Sets the <span class="parameter">$y</span> co-ordinate (ordinate) to output the template. Value should be
  specified as <span class="smallblock">LENGTH</span> in millimetres.

  Default: `null` - Sets <span class="parameter">$y</span> co-ordinate to `0`

  <span class="smallblock">BLANK</span> or omitted - uses default (`0`)

  `-1`: Uses current writing position in document

<span class="parameter">$width</span>

: Specifies the width for the template to appear on the page. Value should be specified as <span class="smallblock">LENGTH</span>
  in millimetres.

  Default or `0` will output the template at the original size (if
  neither <span class="parameter">$width</span> nor <span class="parameter">$height</span> are set) or
  if <span class="parameter">$height</span> is set, the <span class="parameter">$width</span> is automatically
  set to output the template in proportion to the original.

<span class="parameter">$height</span>

: Specifies the height for the template to appear on the page. Value should be specified as <span class="smallblock">LENGTH</span>
  in millimetres.

  Default or `0` will output the template at the original size (if
  neither <span class="parameter">$width</span> nor <span class="parameter">$height</span> are set) or
  if <span class="parameter">$width</span> is set, the <span class="parameter">$height</span> is automatically
  set to output the template in proportion to the original.

## Return Value

`UseTemplate()` returns an array of the calculated <span class="parameter">$width</span> and <span class="parameter">$height</span>.

# Changelog

<table class="table">
<thead>
<tr>
  <th>Version</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>2.3</td>
  <td>Function was added.</td>
</tr>
<tr>
  <td>8.0</td>
  <td>Function was removed in favour of <a href="{{ "/reference/mpdf-functions/usetemplate-v8.html" | prepend: site.baseurl }}">useTemplate()</a>.</td>
</tr>
</tbody>
</table>

# Examples

Example #1

```php
<?php
// Require composer autoload
require_once __DIR__ . '/vendor/autoload.php';
$mpdf = new \Mpdf\Mpdf();
$mpdf->SetImportUse();

// Add First page
$mpdf->AddPage();

$pagecount = $mpdf->SetSourceFile('logoheader.pdf');
$tplId = $mpdf->ImportPage($pagecount);
$actualsize = $mpdf->UseTemplate($tplId);

// The height of the template as it was printed is returned as $actualsize['h']
// The width of the template as it was printed is returned as $actualsize['w']
$mpdf->WriteHTML('Hello World');

$mpdf->Output();

```


Example #2 - Using a 'cropped' page

```php
<?php
// Require composer autoload
require_once __DIR__ . '/vendor/autoload.php';

$mpdf = new \Mpdf\Mpdf();
$mpdf->SetImportUse();

$pagecount = $mpdf->SetSourceFile('testfile.pdf');
$tplId = $mpdf->ImportPage($pagecount, 50, 50, 100, 100);
$mpdf->UseTemplate($tplId, '', '', 100, 100);

$mpdf->Output();

```

# See Also

* <a href="{{ "/reference/mpdf-functions/setimportuse.html" | prepend: site.baseurl }}">SetImportUse()</a> - Enable the use of imported PDF files or templates
* <a href="{{ "/reference/mpdf-functions/thumbnail.html" | prepend: site.baseurl }}">Thumbnail()</a> - Print thumbnails of an external PDF file
* <a href="{{ "/reference/mpdf-functions/setsourcefile.html" | prepend: site.baseurl }}">SetSourceFile()</a> - Specify the source PDF file used to import pages into the document
* <a href="{{ "/reference/mpdf-functions/importpage.html" | prepend: site.baseurl }}">ImportPage()</a> - Import a page from an external PDF file
* <a href="{{ "/reference/mpdf-functions/setpagetemplate.html" | prepend: site.baseurl }}">SetPageTemplate()</a> - Specify a page from an external PDF file to use as a template
* <a href="{{ "/reference/mpdf-functions/setdoctemplate.html" | prepend: site.baseurl }}">SetDocTemplate()</a> - Specify an external PDF file to use as a template
* <a href="{{ "/reference/mpdf-functions/restartdoctemplate.html" | prepend: site.baseurl }}">RestartDocTemplate()</a> - Re-start the use of a Document template from the next page<

