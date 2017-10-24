# Upload Library for Google Storage

How to use it:

Creating the Upload Form

Using a text editor, create a form called upload_form.php. In it, place this code and save it to your application/views/ directory:

<html>
<head>
<title>Upload Form</title>
</head>
<body>

<?php echo $error;?>

<?php echo form_open_multipart( $upload_url );?>

<input type="file" name="userfile" size="20" />

<br /><br />

<input type="submit" value="upload" />

</form>

</body>
</html>



#The Controller

Using a text editor, create a controller called Upload.php. In it, place this code and save it to your application/controllers/ directory:

<?php

class Upload extends CI_Controller {

        public function __construct()
        {
                parent::__construct();
                $this->load->helper(array('form', 'url'));

                $config['gs_bucket_path'] = 'gs://YOURAPP.appspot.com/uploads';
        
                $config['allowed_types'] = 'csv';
                $config['max_size']	= '10000000';
              
                $this->load->library('upload', $config);
        }

        public function index()
        {
                $this->load->view('upload_form', array(
                    'error' => ' ' ,
                    'upload_url' => $this->upload->get_upload_url("/upload_handle")
                    ));
        }

        public function upload_handle()
        {
                $this->load->library('upload', $config);

                if ( ! $this->upload->do_upload('userfile'))
                {
                        $error = array('error' => $this->upload->display_errors());

                        $this->load->view('upload_form', $error);
                }
                else
                {
                        $data = array('upload_data' => $this->upload->data());

                        $this->load->view('upload_success', $data);
                }
        }
}
?>

