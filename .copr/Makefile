# 1. Spec dosyasindan versiyonu otomatik cek
VERSION := $(shell grep -m1 "%define specrpmversion" specs/kernel.spec | awk '{print $$3}')

srpm:
	# Gerekli araclari kur
	dnf install -y rpmdevtools wget

	# Dosyaları hazirlik dizinine kopyala
	cp configs/* .
	cp scripts/* .
	cp certs/* .
	cp specs/*.changelog . || true
	cp specs/Module.kabi_* . || true

	# Artik $(VERSION) sayesinde versiyonu spec dosyasindan otomatik cekiyor.
	wget -N https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-$(VERSION).tar.xz

	# SRPM OLUŞTUR
	rpmbuild -bs \
		--define "_sourcedir $(PWD)" \
		--define "_srcrpmdir $(outdir)" \
		specs/kernel.spec
