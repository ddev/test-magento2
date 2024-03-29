# test-magento2 for DDEV testing only

To recreate:

1. Do the normal quickstart setup for magento2, but use `ddev composer install --no-dev`
2. Log in to the site as admin using admin credentials and the admin URL listed in etc/app/env.php
3. Catalog->Products->Add Product->Simple Product
4. Create a project called unicycle with a picture `unicycle.jpg` from https://m.media-amazon.com/images/I/715LWvYxcLL._AC_SY879_.jpg or anywhere else.

    ```bash
    echo "This is a junk" >pub/junk.txt
    VERSION=2.4.6-p4
    mkdir -p .tarballs
    tar -czf .tarballs/testpkgmagento2_code_no_media.magento${VERSION}.tgz --exclude=.ddev --exclude=.git --exclude=dev --exclude=var --exclude=pub/media --exclude=.tarballs --exclude=app/etc/env.php .
    ddev export-db --gzip=false --file=.tarballs/db.sql && tar -czf .tarballs/testpkgmagento2.magento${VERSION}.db.tgz -C .tarballs db.sql
    tar -czf .tarballs/testpkgmagento2_files.magento2.4.6-p4.tgz -C pub/media .
    
    ```

5. Create an empty release with the VERSION you have used
6. Add the tarballs from .tarballs to it. The `gh` GitHub command-line tool is required:

```bash
gh release upload ${VERSION} .tarballs/*gz
```
