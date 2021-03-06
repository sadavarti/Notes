# Install Latest Release of [OpenSSL](https://www.openssl.org/) from Source on CentOS

## Install Dependencies
* "Development Tools"

      # Find the name of "Development Tools" Group under "Optional Group" in the output
      # Pass the group name to `yum group install`
      # The group name may be translated string(e.g. "开发工具" in Chinese)
      yum group list
      sudo yum group install -y "Development Tools"

* [zlib](https://www.zlib.net/)
   * [Install zlib on CentOS from Source](https://github.com/northbright/Notes/blob/master/zlib/install-zlib-on-centos-from-source.md) 

## Download Latest Source on [github](https://github.com/openssl/openssl/releases) or [offical site](https://www.openssl.org/source/)

    wget https://github.com/openssl/openssl/archive/OpenSSL_1_1_1b.tar.gz
    tar -xzvf OpenSSL_1_1_1b.tar.gz
    cd openssl-OpenSSL_1_1_1b
            
## Configure, Make and Install
```
# configure
# use `rpath` to make sure our latest OpenSSL libs are loaded
# it won't upgrade system default OpenSSL libs 
# to avoid incompatible issues(ldconfig)
# OpenSSL links zlib statically by default

LDFLAGS="-Wl,-rpath=/usr/local/openssl/lib" \
\
./config --prefix=/usr/local/openssl \
--openssldir=/usr/local/openssl \
--with-zlib-include=/usr/local/zlib/include \
--with-zlib-lib=/usr/local/zlib/lib \

make
sudo make install
```

## Check `rpath` in binary and libraries using `readelf`
```
readelf -d /usr/local/openssl/bin/openssl
readelf -d /usr/local/openssl/lib/libssl.so
readelf -d /usr/local/openssl/lib/libcrypto.so
```

## Check linked libraries using `ldd`
```
ldd /usr/local/openssl/bin/openssl
ldd /usr/local/openssl/lib/libssl.so
ldd /usr/local/openssl/lib/libcrypto.so
```

## Add New Binary Path of OpenSSL(Optional)
* `sudo vi /etc/profile`

      # Append these lines:
      # Use New Version of OpenSSL
      export PATH=/usr/local/openssl/bin:$PATH

* `source /etc/profile`

## Add New Binary Path of OpenSSL for `sudo`(Optional)
```
su
visudo

# Find "secure_path", insert "/usr/local/openssl/bin"
Defaults    secure_path = /usr/local/openssl/bin:/sbin:/bin:/usr/sbin:/usr/bin
```
        
## Check OpenSSL Version
   
    openssl version -a

## References
* [How to install latest version of OpenSSL on CentOS?](https://blacksaildivision.com/how-to-install-openssl-on-centos)
