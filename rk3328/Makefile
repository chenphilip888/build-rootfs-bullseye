export BOARD=rk3328
export ARCH=arm64
export LB_BOOTSTRAP_INCLUDE="apt-transport-https gnupg"

all: clean prebuild buildrootfs postbuild mkimage

clean:
	sudo lb clean --purge
	\rm -rf config

prebuild:
	lb config \
	--apt-indices false \
	--apt-recommends false \
	--apt-secure false \
	--architectures arm64 \
	--archive-areas 'main contrib non-free' \
	--backports false \
	--binary-filesystem ext4 \
	--binary-images tar \
	--bootappend-live "hostname=linaro-alip username=linaro" \
	--bootstrap-qemu-arch arm64 \
	--bootstrap-qemu-static /usr/bin/qemu-aarch64-static \
	--cache false \
	--chroot-filesystem none \
	--compression gzip \
	--debootstrap-options "--variant=minbase --include=apt-transport-https,gnupg" \
	--distribution bullseye \
	--gzip-options '-9 --rsyncable' \
	--iso-publisher 'Linaro; http://www.linaro.org/; linaro-dev@lists.linaro.org' \
	--iso-volume 'Linaro Bullseye $(date +%Y%m%d-%H:%M)' \
	--linux-flavours none \
	--linux-packages none \
	--mode debian \
	--security true \
	--system normal \
	--updates true

buildrootfs:
	\cp -rf $(PWD)/../customization/* config/
	sudo lb build

postbuild:
	$(PWD)/../mk-rootfs-bullseye.sh

mkimage:
	$(PWD)/../mk-image.sh
