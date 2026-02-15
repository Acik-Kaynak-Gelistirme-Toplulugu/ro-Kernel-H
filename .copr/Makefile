# .copr/Makefile
SPEC_VER := $(shell grep -m1 "%define specrpmversion" specs/kernel.spec | awk '{print $$3}')
BASE_VER := $(shell echo $(SPEC_VER) | cut -d. -f1,2)

# Fedora Resmi Dist-Git URL'si (Herkese Açık)
FEDORA_URL := https://src.fedoraproject.org/rpms/kernel/raw/rawhide/f

srpm:
	# 1. gerekli araclari kur
	dnf install -y rpmdevtools wget awk grep

	# 2. dosyalari hazirla
	cp configs/* .
	cp scripts/* .
	cp certs/* .
	cp specs/* . || true
	mkdir -p patches

	# 3. tarball kernel
	wget -N https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-$(SPEC_VER).tar.xz

	# 4. eksik dosyalar
	for file in mod-denylist.sh def_variants.yaml.rhel def_variants.yaml.fedora \
	           generate_all_configs.sh kernel.sbat.template kvm_stat.logrotate \
	           kernel-local README.rst rpminspect.yaml x509.genkey.fedora; do \
		wget -N $(FEDORA_URL)/$$file || echo "$$file bulunamadı, devam ediliyor..."; \
	done

	# 5. yamalari indir
	wget -N $(FEDORA_URL)/patch-$(BASE_VER)-redhat.patch || echo "Redhat patch cannot found."
	wget -N $(FEDORA_URL)/linux-kernel-test.patch || echo "Test patch cannot found."
	wget -N $(FEDORA_URL)/kernel-abi-stablelists-$(SPEC_VER).tar.xz || echo "kABI list cannot found."

	# 6. SRPM olustur
	rpmbuild -bs \
		--define "_sourcedir $(PWD)" \
		--define "_srcrpmdir $(outdir)" \
		kernel.spec
