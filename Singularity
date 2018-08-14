Bootstrap: docker
From: centos:latest

%labels
Maintainer by Emanuel Schmid @ VITAL-IT
Version v2.3.0.140936


%help
This is the old PacBio collection of tools as in smrtlink version V2.3.0 for RSII data
        
%post

# install software
yum update -y -q && yum install -y -q \
        build-essential \
        gcc-multilib \
        libboost-all-dev \
        libhdf5-serial-dev \
        zlib1g-dev \
        pkg-config \
        wget \
        rsync \
        unzip \
        which \
        bzip2 \
        dirname
    


echo "add new user if not existent"
        
SMRT_USER=smrtanalysis
if ! grep -c "smrtanalysis:" /etc/passwd
then
        useradd  -g users -d /home/$SMRT_USER -s /bin/bash -p PacBio $SMRT_USER
else
        echo    "user already exists"
fi

echo "generate a new PacBio root directory and make smrtuser owner"

SMRT=/opt/pacbio
if [ ! -d $SMRT ]
then
        mkdir $SMRT
        chown smrtanalysis:users $SMRT
fi

echo "now switch to smrt-user"

su $SMRT_USER
SMRT=/opt/pacbio
SMRT_ROOT="/opt/pacbio/smrtlink"
cd $SMRT

echo "download and extract smrtlink"
wget -c https://downloads.pacbcloud.com/public/software/installers/smrtanalysis_2.3.0.140936.run --no-check-certificate
chmod +x smrtanalysis_2.3.0.140936.run 

if [ -d $SMRT_ROOT ] 
then 
        rm -rf $SMRT_ROOT
        ./smrtanalysis_2.3.0.140936.run --batch --rootdir $SMRT_ROOT --ignore-syscheck --extract-only   
else 
        ./smrtanalysis_2.3.0.140936.run --batch  --rootdir $SMRT_ROOT --ignore-syscheck --extract-only
fi

echo "cleaning up"
rm smrtanalysis_2.3.0.140936.run
%environment
export PATH=/opt/pacbio/smrtlink/install/smrtanalysis_2.3.0.140936/analysis/bin/:$PATH
%runscript
exec /bin/bash "$@"
