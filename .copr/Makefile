# .copr/Makefile

# 1. Spec dosyasindan tam versiyonu al
SPEC_VER := $(shell grep -m1 "%define specrpmversion" specs/kernel.spec | awk '{print $$3}')

# 2. Ana versiyonu al
BASE_VER := $(shell echo $(SPEC_VER) | cut -d. -f1,2)

# Fedora GitLab URL
ARK_URL := https://gitlab.com/ckievit/kernel-ark/-/raw/os-build

srpm:
	dnf install -y rpmdevtools wget awk grep

	# Dosyalari duzelt
	cp configs/* .
	cp scripts/* .
	cp certs/* .
	cp specs/* . || true
	# Varsa yerel yamalari kopyala
	mkdir -p patches
	cp patches/* . || true



	# Ana Kernel Tarball
	wget -N https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-$(SPEC_VER).tar.xz

	# Red Hat Yamasi
	wget -N $(ARK_URL)/patch-$(BASE_VER)-redhat.patch || echo "Redhat patch cannot found"

	# kABI Stablelists
	wget -N $(ARK_URL)/kernel-abi-stablelists-$(SPEC_VER).tar.xz || echo "kABI cannot found."

	# Test Yamasi
	wget -N $(ARK_URL)/linux-kernel-test.patch || echo "Test patch cannot found."

	# SRPM olustur
	rpmbuild -bs \
		--define "_sourcedir $(PWD)" \
		--define "_srcrpmdir $(outdir)" \
		kernel.spec
