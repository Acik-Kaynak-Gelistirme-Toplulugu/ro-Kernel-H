# .copr/Makefile
SPEC_VER := $(shell grep -m1 "%define specrpmversion" specs/kernel.spec | awk '{print $$3}')
FEDORA_URL := https://src.fedoraproject.org/rpms/kernel/raw/rawhide/f

srpm:
	dnf install -y rpmdevtools wget awk grep

	cp configs/* .
	cp scripts/* .
	cp certs/* .
	cp specs/* . || true
	cp patches/* . || true

	wget -N https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-$(SPEC_VER).tar.xz

	for file in mod-denylist.sh def_variants.yaml.rhel def_variants.yaml.fedora \
	           generate_all_configs.sh kernel.sbat.template kvm_stat.logrotate \
	           kernel-local README.rst rpminspect.yaml x509.genkey.fedora; do \
		wget -N $(FEDORA_URL)/$$file || echo "$$file bulunamadı, devam ediliyor..."; \
	done

	rpmbuild -bs \
		--define "_sourcedir $(PWD)" \
		--define "_srcrpmdir $(outdir)" \
		kernel.spec
