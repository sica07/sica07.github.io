# ImageMagick

# PDF not quthorized error when using `convert`
Make the following changes in the file _/etc/ImageMagick-6/policy.xml_:
```
 <!-- disable ghostscript format types -->
  <!--
  <policy domain="coder" rights="none" pattern="PS" />
  <policy domain="coder" rights="none" pattern="EPI" />
  <policy domain="coder" rights="none" pattern="PDF" />
  <policy domain="coder" rights="none" pattern="XPS" />
  -->
  <policy domain="coder" rights="read|write" pattern="PDF,PS" />
</policymap>
```
[src](https://cromwell-intl.com/open-source/pdf-not-authorized.html)


