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

	for file in mod-denylist.sh def_variants.yaml.rhel  \
	           kernel.sbat.template kvm_stat.logrotate \
	           README.rst rpminspect.yaml x509.genkey.fedora merge.py; do \
		wget -N $(FEDORA_URL)/$$file || echo "$$file bulunamadı."; \
	done

	chmod +x *.sh *.py

	rpmbuild -bs \
		--define "_sourcedir $(PWD)" \
		--define "_srcrpmdir $(outdir)" \
		kernel.spec
