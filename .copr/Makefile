srpm:
	# Gerekli araçları yükle
	dnf install -y rpmdevtools fedpkg

	# Derleme için dosyaları kök dizine (geçici olarak) topla
	cp specs/kernel.spec .
	cp specs/sources .
	cp specs/*.changelog .
	cp configs/* .
	cp scripts/* .
	cp certs/* .
	cp Module.kabi_* . || true

	# SRPM (Kaynak paketini) oluştur ve COPR'un istediği yere koy
	fedpkg --dist rawhide srpm --outdir $(outdir)
