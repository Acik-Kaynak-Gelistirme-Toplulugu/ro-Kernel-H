# .copr/Makefile
SPEC_VER := $(shell grep -m1 "%define specrpmversion" specs/kernel.spec | awk '{print $$3}')

srpm:
	dnf install -y rpmdevtools wget awk grep

	cp configs/* .
	cp scripts/* .
	cp certs/* .
	cp specs/* . || true
	cp patches/* . || true


	wget -N https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-$(SPEC_VER).tar.xz

	rpmbuild -bs \
		--define "_sourcedir $(PWD)" \
		--define "_srcrpmdir $(outdir)" \
		kernel.spec
