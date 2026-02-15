srpm:
	dnf install -y rpmdevtools fedpkg

	cp specs/kernel.spec .
	cp specs/sources .
	cp specs/*.changelog .
	cp configs/* .
	cp scripts/* .
	cp certs/* .
	cp Module.kabi_* . || true

	fedpkg --dist rawhide srpm --outdir $(outdir)
