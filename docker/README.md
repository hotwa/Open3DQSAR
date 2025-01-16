
```shell
wget https://registrationcenter-download.intel.com/akdlm/IRC_NAS/dfc4a434-838c-4450-a6fe-2fa903b75aa7/intel-oneapi-base-toolkit-2025.0.1.46_offline.sh
sh ./intel-oneapi-base-toolkit-2025.0.1.46_offline.sh -a --silent --cli --eula accept
./configure CC=icc --with-intel-mkl && make && make install
```

error:
```shell
root@efb88430e4ba:/Open3DQSAR/open3dqsar# ./configure CC=icc --with-intel-mkl
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a race-free mkdir -p... /usr/bin/mkdir -p
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu
checking whether make supports the include directive... yes (GNU style)
checking for gcc... icc
checking whether the C compiler works... no
configure: error: in `/Open3DQSAR/open3dqsar':
configure: error: C compiler cannot create executables
See `config.log' for more details
```