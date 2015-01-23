# multicabinet-deb
## Infrastructure for building and uploading multicabinet deb packages
1. Packaging instructions
  1. Increment build version
  ```
  dch -v 0.2-3 -D stable
  ```
  2. Actual build phase
  ```
  debuild -us -uc -i -I -b
  ```
2. Uploading instructions
  1. Include .changes file
  ```
  cd ~/repository/apt/
  reprepro -Vb . include wheezy ~/multicabinet/multicabinet2_0.2-3_i386.changes
  ```
  2. Synchronize repository
  ```
  rsync --delete -avhe ssh ../apt/ packagebuild@private.netdedicated.ru:www/repo.multicabinet.com/ROOT/debian
  ```


