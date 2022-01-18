# vagrant-mikrok8s

Sistemimizden izole bir geliştirme ortamına ihtiyaç duyduğumuzda imdadımıza koşan Vagrant kısaca 
istediğimiz otamı istediğimiz içerik ile virtualbox üzerine hızlıca kurmamızı sağlar.

Bir adet Vagrantfile oluşturur ve içerisinde istediğimiz konfigürasyonu yaparız. Tek bir komut 
ile kurulumu tamamlarız.

Örnek olarak aşağıdaki Vagrantfile ile bir ubuntu sanal makinası ayağa kaldıralım. 

Gereksinimler:

    Vagrant: https://www.vagrantup.com/downloads
    Virtualbox: https://www.virtualbox.org/wiki/Downloads


Adımlar:

    Ekteki dosya bir klasöre çıkartılır.
    Konsol açılır ve Vagrantfile ın olduğu klasöre gidilir.
    'vagrant up' komutu çalıştırılır.
    'ssh vagrant@192.168.51.10' komutu ile ssh üzerinden oluşturulan ve ayağa kalkan makinaya bağlanılır.


Sonuç olarak bir node ile ayağa kalktı sistemimiz:

	vagrant@ubuntu2004:~$ kubectl get nodes
	NAME                     STATUS   ROLES    AGE   VERSION
	ubuntu2004.localdomain   Ready    <none>   11h   v1.22.4-3+adc4115d990346

Namespacelerimiz:

	vagrant@ubuntu2004:~$ kubectl get ns
	NAME              STATUS   AGE
	kube-system       Active   10h
	kube-public       Active   10h
	kube-node-lease   Active   10h
	default           Active   10h
	metallb-system    Active   10h

Ayrıca dashboard a girmek için 'microk8s dashboard-proxy' komutu kullanılabilir. Tarayıcımızdan girebilmek için Vagrantfile da tanımlamasını yaptığımız private_network ip adresi ve aşağıda gözüken port girilmelidir(https://192.168.51.10:10443). Eğer doğru adres girildiği halde bağlanmıyorsa Virtualbox üzerinden bağlantı noktası yönlendirme kuralı eklemek gerekebilir. Konsolda verilen token ile dashboard a giriş yapılır.

	vagrant@ubuntu2004:~$ microk8s dashboard-proxy
	Checking if Dashboard is running.
	Dashboard will be available at https://127.0.0.1:10443
	Use the following token to login:
	eyJhbGciOiJSUzI1NiIsImtpZCI6IjJWUVNvSnFWVjJlQTlVWl9tSEtPaG1KYnNtZ1pHYzRFcEZwQmtsWmRZdTAifQ.eyJpc3MiOiJrdWJlcm...

Yararlı komutlar:

	vagrant up                         -> vagrant ile makina oluşturur
	vagrant global-status              -> vagrant global durumunu verir
	vagrant halt {vagrant id}          -> kapatır
	vagrant reload {vagrant id}        -> yeniden başlatır
	vagrant suspend {vagrant id}       -> uykuya alır
	vagrant resume {vagrant id}        -> uykudan uyandırır
	vagrant ssh
	vagrant destroy {vagrant id}       -> makinayı yokeder
