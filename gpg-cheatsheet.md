
GnuPG CheatSheet
===================

| [Anahtar Çifti Oluşturma](#gen-key) | [Anahtar Yönetimi](#key-management) | [İmza İşlemleri](#sign) | [Şifreleme/Deşifre Etme](#encrypt-decrypt) | [Anahtar Sunucusu](#keyserver) | <br> | [Anahtar Düzenleme](#edit-key) | [Akıllı Kartlar](#smart-cards) | [İpuçları](#tips) |  [Katkı](#contributor) | [Lisans](#license) | [Referans](#ref) | 

<pre> _____ _____ _____    _____ _           _       _           _   
|   __|  _  |   __|  |     | |_ ___ ___| |_ ___| |_ ___ ___| |_ 
|  |  |   __|  |  |  |   --|   | -_| .'|  _|_ -|   | -_| -_|  _|
|_____|__|  |_____|  |_____|_|_|___|__,|_| |___|_|_|___|___|_|  
</pre>

<a id="gen-key"></a>
#### Anahtar Çifti Oluşturma
<code>gpg --gen-key</code>

Anahtar çifti oluşturmak için kullanılır.

**not**: Bu noktadan sonra GnuPG, anahtar çiftinin nasıl oluşturulacağı ile ilgili seçenekler listeler.
2 . adımda satır boş bırakılırsa gpg anahtarı öntanımlı olarak 2048 bit halde oluşturacaktır.
Daha güvenli bir anahtar için 4096 bit seçilmeli. 3. adımda bulunan geçerlilik tarihi için ise;
1 veya 2 yıldan daha uzun bir süre seçmemek yine avantajlı olacaktır.
\------------------------------------------------------------------------------------

<code>gpg -ao iptal_sertifika.asc --gen-revoke key_id</code>

id'si belirtilen anahtar için, örn. "iptal_sertifika.asc" olarak ASCII zırlı biçimde iptal sertifikası oluşturur.
\------------------------------------------------------------------------------------

<code>gpg --import iptal_sertifika.asc </code>

örn. "iptal_sertifika.asc" adındaki iptal sertifikasını içe aktarır, böylece hangi anahtar için oluşturulduysa onu iptal eder.   

**not 0**: İptal sertifikasını harici bir depolama alanında saklamak daha doğru olacaktır. <br />
**not 1**: Herhangi bir nedenle anahtarınızı iptal ettiyseniz, anahtarı kullanacak olan kişilerin de bunu bilmesi gerekir. Eğer sunucu kullanıyorsanız, iptal etmiş olduğunuz anahtarı tekrar 
sunucuya göndererek ([--send-keys](#send-keys)) bunu sağlayabilirsiniz.
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 
#### Anahtar Yönetimi<a id="key-management"></a>

<code>gpg --list-secret-keys</code>

Kayıtlı özel anahtarları ve ilgili ayrıntıları (key id, son kullanma tarihi, vs.) listeler
\------------------------------------------------------------------------------------

<code>gpg --list-keys</code>

Kayıtlı genel anahtarları ve ilgili aytıntıları (key id, son kullanma tarihi, vs.) listeler
\------------------------------------------------------------------------------------

<code>gpg --fingerprint</code>

Anahtarlara ait parmak izlerini (fingerprint) listeler.<br />
\------------------------------------------------------------------------------------

<code>gpg --delete-secret-key "kullanici_adi/key_id"</code>

Belirtilen key id veya kullanıcı adına ait özel anahtarı siler.<br />
\------------------------------------------------------------------------------------

<code>gpg --delete-key "kullanici_adi"</code>

Belirtilen key id veya kullanıcı adına ait genel anahtarı siler.

**not:** Bu komut belirtilen kullanıcı adına ait bir özel anahtar olmadığında kullanılabilir.
Aksi ihtimalde önce bir üstte bulunan komutu kullanarak özel anahtarı silmeniz gerekir.<br />
\------------------------------------------------------------------------------------

<code>gpg -ao ozel_anahtar.asc --export-secret-keys "anahtar_id"</code>

id'si belirtilen özel anahtarı, örn. "anahtar.asc" olarak ASCII zırhlı ve açık biçimde dışa aktarır.
\------------------------------------------------------------------------------------

<code>gpg -a --export-secret-keys "anahtar_id" | gpg -aco ozel_anahtar.gpg.asc</code>

id'si belirtilen özel anahtarı  örn. "ozel_anahtar.gpg.asc" olarak, ASCII zırhlı
biçimde şifreli olarak dışa aktarır. Komutun ilk kısmı ise özel anahtarı ekrana yazdırır.<br />
\------------------------------------------------------------------------------------

<code>gpg -ao genel_anahtar.asc --export "anahtar_id"</code>

id'si ya da kullanıcı adı belirtilen genel anahtarı örn. "genel_anahtar.asc" olarak ASCII zırhlı biçimde dışa aktarır.
\------------------------------------------------------------------------------------

<code>gpg --allow-secret-key-import --import ozel.anahtar</code>

örn. "ozel.anahtar" adındaki özel anahtarı içe aktarır.<br />
\------------------------------------------------------------------------------------

<code>gpg --import genel.anahtar</code>

örn. "genel.anahtar" adındaki genel anahtarı içe aktarır.<br />
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### İmza İşlemleri<a id="sign"></a>

<code> gpg --clearsign mesaj.txt</code>

İmzayı metin/mesaj dosyasının içine yazdırır. 
örn. "mesaj.txt" dosyasını "mesaj.txt.asc" olarak ASCII zırlı biçimde dışarı aktarır. 

<code>gpg --verify mesaj.txt.asc</code>

Çıktı olarak; imzanın tarih detaylarını, imzalayanın anahtar id'sini ve doğrulama bilgisi ile birlikte imzalayanın kullanıcı bilgilerini verir.<br />
\------------------------------------------------------------------------------------

<code>gpg --armor --detach-sign dosya.*</code>

örn. "dosya.* " dosyasını imzalar, imza dosyasını "dosya.*.asc" olarak ASCII zırlı biçimde dışarı aktarır.

<code>gpg --armor --detach-sign dosya.\*.asc  dosya.*</code>

Çıktı olarak; imzanın tarih detaylarını, imzalayanın anahtar id'sini ve doğrulama bilgisi ile birlikte imzalayanın kullanıcı bilgilerini verir.<br />
\------------------------------------------------------------------------------------

<code>gpg --detach-sign dosya.*</code>

örn. "dosya.* " dosyasını imzalar ve imza dosyasını "dosya.*.sig" olarak dışarı aktarır.

<code>gpg --detach-sign dosya.\*.sig dosya.*</code>

Çıktı olarak; imzanın tarih detaylarını, imzalayanın anahtar id'sini ve doğrulama bilgisi ile birlikte imzalayanın kullanıcı bilgilerini verir.   <br />
\------------------------------------------------------------------------------------

<code>gpg --output dosya.sig --sign dosya</code>

örn. dosyayı imzalar ve "dosya.sig" olarak dışarı aktarır.

<code>gpg --output dosya --decrypt dosya.sig</code>

Çıktı olarak; imzanın tarih detaylarını, imzalayanın anahtar id'sini ve doğrulama bilgisi ile birlikte imzalayanın kullanıcı bilgilerini verir.   <br />
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Şifreleme-Deşifre Etme<a id="encrypt-decrypt"></a>

<code>gpg -e -u "x@e-posta.com" -r "y@e-posta.com" dosya.*</code>

Gönderen (x@e-posta.com) tarafından, örn. "dosya.\*" dosyasını
alıcı (y@e-posta.com) için şifreler ve dosya.\*.gpg olarak dışarı aktarır.<br />
\------------------------------------------------------------------------------------

<code>gpg -e -u "x@e-posta.com" -r "y@e-posta.com" --sign dosya.*</code>

Gönderen (x@e-posta.com) tarafından, örn. "dosya.\*" dosyasını 
alıcı (y@e-posta.com) için şifreler ve imzalar. dosya.\*.gpg olarak dışarı aktarır.

<code>gpg -e -r y@e-posta.com --sign dosya.*</code>

Öntanımlı özel anahtarınız ile örn. "dosya.*" dosyasını, alıcı (y@e-posta.com) için şifreler.<br />

\------------------------------------------------------------------------------------

<code>gpg -d dosya.txt.gpg</code>

Şifreli dosyayı (örn. dosya.txt.gpg) deşifre eder ve  yazdırır.<br />
\------------------------------------------------------------------------------------

<code>gpg -o dosya.* -d dosya.*.gpg</code>

Şifreli dosyayı (örn. dosya.*.gpg) deşifre eder ve belirtilen ad-tür ile dışa aktarır.<br />
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Anahtar Sunucusu<a id="keyserver"></a>

<a id="send-keys"></a><code>gpg --keyserver sunucu_url --send-keys key_id</code>

id'si belirtilen anahtarı istenen anahtar sunucusuna gönrerir.<br />
\------------------------------------------------------------------------------------

<code>gpg --keyserver sunucu_url --recv-key key_id</code>

id'si belirtilen anahtarı istenen anahtar sunucusundan çeker. <br />
\------------------------------------------------------------------------------------

<code>gpg --keyserver sunucu_url  --search-keys arama\_parametresi</code>

Tercih edilen arama parametresine göre, belirtilen anahtar sunucusundan, genel anahtarı/anahtarları numaralandırarak listeler. Girilen numara ile istenen anahtarı çeker. <br />
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
        
#### Anahtar Düzenleme<a id="edit-key"></a>

<code>gpg --edit-key key_id</code>

id'si belirtilen anahtar ile ilgili belirli düzenlemeler yapmak için kullanılır.

Bu noktada id'si belirtilen anahtar için aşağıdaki komutlar uygulanarak bir takım düzenlemeler yapılabilir.
Eğer bir anahtar sunucusu kullanılıyorsa, **genel anahtarlar** (duruma göre sizin veya başkasının) üzerinde yapmış olduğunuz değişikliklerin bilinmesi için kaydettikten sonra ([save](#save))  tekrar sunucuya göndermeniz ([--send-keys](#send-keys)) gerekir. <br />
\------------------------------------------------------------------------------------

<code>passwd</code>

Belirtilen özel anahtarın passphrase(parola)'ini değiştirmek için kullanılır.<br />
\------------------------------------------------------------------------------------

<code>sign</code> 

Belirtilen anahtarı imzalamak için kullanılır.

**not**: İmzaladığınız anahtar; ana anahtarınız, size ait bir alt-anahtar veya bir başkasının anahtarı olabilir.  Bir başkasının anahtarını imzaladığınız durumda, kaydedip ([save](#save)) anahtarı sunucuya geri gönderirseniz 
([--send-keys](#send-keys)), bu imza anahtara ait ayrıntılarda görüntülenecektir. Böylece anahtar üzerinde belitrilen hesabın doğru yani e-posta hesabının ve adın gerçek sahibine ait olduğuna dair bir referans oluşturacak.  Bu nedenle; **kişisel olarak  tanışıp, mevcut anahtarın tanıştığınız kişiye ait olduğunu doğrulamadığınız kimselerin anahtarlarını imzalamayınız.**
\------------------------------------------------------------------------------------

<code>expire</code>

Anahtarınızın/anahtarlarınızın kullanım tarihini değiştirmek için kullanılır.  <br />
\------------------------------------------------------------------------------------

<code>addphoto</code>

Belirtilen anahtara görsel eklemek için kullanılır.

**not 1** : Görsel jpg formatında ve 240x288px'den büyük olmamalıdır. <br />
**not 2**: Her anahtar sunucuları bu biçimdeki anahtarları kabul etmez.<br />
\------------------------------------------------------------------------------------

<code>showphoto</code>

Eğer anahtara görsel eklenmişse bu görseli görüntülemek için kullanılır.<br />
\------------------------------------------------------------------------------------

<a id="save"></a><code>save</code>

Değişiklikleri kaydedip çıkmak için kullanılır.<br />
\------------------------------------------------------------------------------------

<code>quit</code>

Değişiklikleri kaydetmeden çıkmak için kullanılır.<br />
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Akıllı Kartlar [[k2](#cont-2)] <a id="smart-cards"></a>

<code>gpg --card-edit</code>

Cihaza bağlı olan (örn. yubikey, vb.) akıllı kartlara müdahale etmenizi sağlar.<br />

**not**: Bu noktada gpg aracı, bağlı olan akıllı kartla iletişime girer. Ardından aşağıdaki
komutlar uygulanarak kart üzerinde düzenlemeler yapılır. <br />
\------------------------------------------------------------------------------------

<code>admin</code>

Kart menüsü içerisinde, yönetici haklarına erişim sağlayarak, akıllı kart içindeki "genel anahtar bağlantsı", "kart sahibi" gibi bilgileri düzenlemek için kullanılır.<br />
\------------------------------------------------------------------------------------

<code>name</code>

Akıllı kart sahibinin, ad-soyad bilgilerini düzenlemek için kullanılır.<br />
\------------------------------------------------------------------------------------

<code>url</code>

Akıllı kart içerisinde genel anahtar bağlantısını tanımlamak için kullanılır.<br />
\------------------------------------------------------------------------------------

<code>generate</code>

Yeni anahtar çifti oluşturmak için kullanılır. <br />
\------------------------------------------------------------------------------------

<code>toggle</code>

Listeleme aşamasında genel ve özel anahtarların yer değişimi için kullanılır. <br />
\------------------------------------------------------------------------------------

<code>key numara</code>

Girilen numara ile alt-anahtarlara işlem yapmak için, numarası belirtilen anahtar işaretlenir. 
Örn. sırayla <code>key 1</code>, <code>key 2</code>, <code>key 1</code> derseniz; önce birinci anahtar  ardından 2. anahtar işaretlenir, son olarak 1. anahtarın işareti kalkar.<br />
\------------------------------------------------------------------------------------

<code>keytocard</code>

 Seçilen anahtarların akıllı karta aktarılması için kullanılır.<br />
\------------------------------------------------------------------------------------

<code>save</code>

Değişiklikleri kaydedip çıkmak için kullanılır.<br />
\------------------------------------------------------------------------------------

<code>quit</code>

Değişiklikleri kaydetmeden çıkmak için kullanılır.<br />
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


### İpuçları<a id="tips"></a>

Anahtar çiftinizin oluşturulma sürecinde daha fazla entropy oluşturmak için rng-tools'u<sup>[1](#1)</sup> kullanabilirsiniz. 

örn. 
<code>sudo rngd -r /dev/urandom</code><br />
\------------------------------------------------------------------------------------

[[k1](#cont-1)]Her seferinde anahtar sunucusu belirtmemek ve encoding sorunu yaşamamak için <code>~/.gnupg/gpg.conf </code>dosyasına aşağıdaki eklemeleri yapabilirsiniz.

<code>utf8-strings</code>
<code>keyserver  sunucu_url</code><br />
\------------------------------------------------------------------------------------

[[k1](#cont-1)]Gpg parolanızı(passphrase) belirli bir süre hafızada tutmak için (örn. 12 saat, saniye cinsinden) <code>~/.gnupg/gpg-agent.conf</code> aşağıdaki biçimde ekleme yapabilirsiniz.

<code>
default-cache-ttl 43200<br />
max-cache-ttl 43200<br />
</code><br />
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


#### Referans<a id="ref"></a>
The GNU Privacy Guard Manual: [https://www.gnupg.org/documentation/manuals/gnupg-2.0/](https://www.gnupg.org/documentation/manuals/gnupg-2.0/)

The GnuPG Smartcard HOWTO: [https://www.gnupg.org/howtos/card-howto/en/smartcard-howto.html](https://www.gnupg.org/howtos/card-howto/en/smartcard-howto.html)

<a id="contributor"></a>
#### Katkı
Barış Büyükakyol -- [usrb.in](http://usrb.in) <br>
<a id="cont-1"></a>Samed Beyribey[k1]-- [eventualis.org] (https://eventualis.org)<br>
<a id="cont-2"></a>Arda Kılıçdağı[k2]-- [arda.kilicdagi.com](https://arda.kilicdagi.com)

<a id="license"></a>
#### Lisans

Bu döküman GNU Free Documentation License v1.3 altındadır. 
Lisans metni: [eng](https://www.gnu.org/licenses/fdl.html) / [tr](http://ozgurlisanslar.org.tr/fdl/gnu-free-documentation-license-version-1-3/)

#### Bağlantılar
<a id="1"></a>[1]:  Random Number Generator Tools [https://www.gnu.org/software/hurd/user/tlecarrour/rng-tools.html](https://www.gnu.org/software/hurd/user/tlecarrour/rng-tools.html)
