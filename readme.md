# Convert html to pdf in CodeIgniter
Report is usually use when we want to responsibility a things. On the web we can
generate report in many ways. One of them is convert html to pdf. In PHP, also
we can find out the many library / plugins / helper to do it. Several of them
like FPDF, TCPDF, DOMPDF, Html2Pdf and etc.

# Dompdf - CodeIgniter 3
In this case I would like to share how can I generate report with DOMPDF together
with CodeIgniter 3.x. Currenty the version I'm use is CodeIgniter 3.1.4 and the DOMPDF
is version 0.6.x and 0.8.0

## Dompdf 0.6.x - CodeIgniter 3
You can see the result in production with go to this link: [ci-dompdf6](https://ci-dompdf6.penjelajahtekno.com/report)
and the source code in Github
[ci-dompdf6](https://github.com/satyakresna/ci-dompdf6).

**Setup**
  1. Read my CodeIgniter-Config in Github: [ci-config](https://github.com/satyakresna/catatan-penjelajahtekno/blob/master/codeigniter-config.md).
  2. Change the value of `$config['composer_autoload']` from `$config['composer_autoload'] = FALSE;` to `$config['composer_autoload'] = "./vendor/autoload.php";`
  3. Go to **composer.json** in the root project and attach:
        ```
        {
          "require": {
            "dompdf/dompdf" : "0.6.*"
          }
        }
        ```
        To download dompdf package. Then run `composer update`. Wait a few second and your package is ready.
  4. Create library with the name `Pdfgenerator.php` (you can name it free) and the code contains below:
        ```php
        <?php
        defined('BASEPATH') OR exit('No direct script access allowed');

        define('DOMPDF_ENABLE_AUTOLOAD', false);
        require_once("./vendor/dompdf/dompdf/dompdf_config.inc.php");

        class Pdfgenerator {

          public function generate($html, $filename='', $stream=TRUE, $paper = 'A4', $orientation = "portrait")
          {
            $dompdf = new DOMPDF();
            $dompdf->load_html($html);
            $dompdf->set_paper($paper, $orientation);
            $dompdf->render();
            if ($stream) {
                $dompdf->stream($filename.".pdf", array("Attachment" => 0));
            } else {
                return $dompdf->output();
            }
          }
        }
        ```
  5. Then in your controllers folder create `Report.php` and call your library `$this->load->library('pdfgenerator')`. To generate it call `$this->pdfgenerator->generate(params)`.
    The full code you can see in this [link](https://github.com/satyakresna/ci-dompdf6/blob/master/application/controllers/Report.php).
  6. In step number 5 the `Report controller` call the view `table_report.php` and it will generate to the report preview. The full code you can see in this [link](https://github.com/satyakresna/ci-dompdf6/blob/master/application/views/table_report.php).
  7. **IMAGE ISSUES**. Since dompdf 0.6.x, I cannot call assets like image with `<?php echo base_url(); ?>` (Codeigniter helper url) for load the image to report preview. So, to do that, I do `<?php echo $_SERVER['DOCUMENT_ROOT']."/yourasetsfolder/blabla.jpg or png"` and it works.


## Dompdf 0.8.x - CodeIgniter 3
You can see the result in production with go to this link: [ci-dompdf8](https://ci-dompdf8.penjelajahtekno.com/report)
and the source code in Github
[ci-dompdf8](https://github.com/satyakresna/ci-dompdf8).

**Setup**
  1. Read my CodeIgniter-Config in Github: [ci-config](https://github.com/satyakresna/catatan-penjelajahtekno/blob/master/codeigniter-config.md).
  2. Change the value of `$config['composer_autoload']` from `$config['composer_autoload'] = FALSE;` to `$config['composer_autoload'] = "./vendor/autoload.php";`
  3. Go to **composer.json** in the root project and attach:
        ```
        {
          "require": {
            "dompdf/dompdf" : "^0.8.0"
          }
        }
        ```
        To download dompdf package. Then run `composer update`. Wait a few second and your package is ready.
  4. Create library with the name `Pdfgenerator.php` (you can name it free) and the code contains below:
        ```php
        <?php
        defined('BASEPATH') OR exit('No direct script access allowed');

        require_once("./vendor/dompdf/dompdf/autoload.inc.php");
        use Dompdf\Dompdf;

        class Pdfgenerator {

          public function generate($html, $filename='', $stream=TRUE, $paper = 'A4', $orientation = "portrait")
          {
            $dompdf = new DOMPDF();
            $dompdf->loadHtml($html);
            $dompdf->setPaper($paper, $orientation);
            $dompdf->render();
            if ($stream) {
                $dompdf->stream($filename.".pdf", array("Attachment" => 0));
            } else {
                return $dompdf->output();
            }
          }
        }
        ```
  5. Then in your controllers folder create `Report.php` and call your library `$this->load->library('pdfgenerator')`. To generate it call `$this->pdfgenerator->generate(params)`.
    The full code you can see in this [link](https://github.com/satyakresna/ci-dompdf8/blob/master/application/controllers/Report.php).
  6. In step number 5 the `Report controller` call the view `table_report.php` and it will generate to the report preview. The full code you can see in this [link](https://github.com/satyakresna/ci-dompdf8/blob/master/application/views/table_report.php).
  7. **IMAGE ISSUES**. Since dompdf 0.6.x, I cannot call assets like image with `<?php echo base_url(); ?>` (Codeigniter helper url) for load the image to report preview. So, to do that, I do `<?php echo $_SERVER['DOCUMENT_ROOT']."/yourasetsfolder/blabla.jpg or png"` and it works.

### References
  1. [DomPDF: Image not readable or empty](https://stackoverflow.com/questions/25558449/dompdf-image-not-readable-or-empty)
  2. [Cetak PDF dari HTML pada Codeigniter 3.0](https://agung-setiawan.com/cetak-pdf-dari-html-pada-codeigniter-3-0/)
