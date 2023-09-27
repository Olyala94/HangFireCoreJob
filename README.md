### Hangfire Nedir ve Asp.Net Core’da _Hangfire Kullanımı

Hangfire Nedir?

Hangfire, arkada asenkron olarak çalışan, yani kullanıcıyı bekletmeyen (Background job) olarak tanımladığımız işler için kullanılan bir tooldur.
Quartz.NET ile kıyaslarsak Hangfire’ın kendi arayüzü bulunmaktadır. Hangfire veritabanı üzerinde tablolar oluşturup tablolar üzerinde job’ların job loglarının takibini sağlayabiliyorsunuz. Quartz ise daha basit bir yapıya sahiptir.
Hangfire mutlaka bir depolama alanına, yani DB’ye ihtiyaç duyar. Depolama alanı olarak, birçok veri tabanını destekler (SQL Server, MSMQ, Redis) gibi. Genellikle Time Schedule şeklinde tekrarlayan belli zaman aralıkları ile çalışan işlerde kullanılsalar da birçok farklı kullanım şekilleri vardır.
Neden Hangfire Kullanmalısınız?

Aslında IIS üzerinde belli zaman aralıkları ile çalışan yapılar, Cloud ile daha çok hayatımıza girmiştir. Çünkü, eskiden bu tarz işler için örneğin Windows ortamı için, Windows Servisler, daha sonra örneğin Azure ile cross platform çalışan, Worker Serviceler hayatımıza girmiştir. Ama bu tarz servisler için, sanal bir sunucunun ayağı kaldırılması gerekmektedir. Bu da extra bir maliyete yol açtığı için, IIS üzerinde çalışan Quartz.NET, Hangfire gibi toolara ihtiyaç duyulmuştur.

Hangfire Job Çeşitleri

Enqueue (Fire & Forget): Tetiklendikten sonra bir kez çalışır.

Schedule (Delayed): Belirtilen sürenin sonunda bir kez çalışır

Recurring: Tekrarlı olarak belirli bir zaman aralığında çalışır.

Continuations: Daha önceden tanımlanmış olan job’ın (schedule) başarılı şekilde çalışması durumunda çalışır.

Hangfire Kurulumu

Yapılandırma ayarları program.cs içinde yapılıyor. Uygulama ayağa kalkarken middleware de yazdığımız yapılandırmalar okunacak sonrasında da bu ayarlar kontrol edilecek.

İlk olarak projemize nuget üzerinden Hangfire’ın kütüphanesini ekleyelim. Hangfire.AspNetCore kütüphanesini ekledim ancak Hangfire sadece Asp.Net ile çalışmıyor bunun dışında birçok yazılım teknolojisinde çalışmaktadır.

Ayarlarımı ve loglarımı Sql server veritabanında tutacağım bunun için de Hanfire.SqlServer kütüphanelerimizi ekliyorum.

CronJob verilerimizi bir yerde saklamak için bir veritabanı gerekiyor. Veritabanımı oluşturdum ve appsettings.json dosyasının içinde aşağıdaki gibi sakladım.

Yapılandırma ayarlarımı program.cs üzerinde belirttim. UseSqlServerStorage ile veritabanımı belirttim. Hangfire’ın Recurring job türünü kullanacağım bunun için RecurringJob.AddOrUpdate generic metodunu kullandım. Sınıfımı ve kullanacağım metodu belirttikten sonra cronExpression olarak her gün 21:30 da çalışacak job için expression ayarımı yaptım.

AddHangFireServer ile uygulamamızın çalışacağı server’ı servislerim arasına ekliyorum.

Bu aslında bizim cronServer’ımızdan biridir. Birden fazla uygulama aynı veritabanına bağlanıp birden fazla cronServer oluşturup çalışabilirler.

Hangfire’ın bir dashboard’u var bu dashboard’ı kullanmak için aşağıdaki metodu ekleyebilirsiniz.

Hangfire Kullanımı

Diyelim ki her gün akşam saatlerinde kullanıcılarımıza sitemize eklenen yeni ürünler hakkında maille bildirim yapmak istiyoruz. Yani biz burada sürekli kendini yenileyen bir job istiyoruz. Bu job belirlediğimiz şartları sağladığında çalışacaktır. Örnek class’ımız aşağıdadır:

Uygulamamızı ayağa kaldırdığımızda veritabanımızda ihtiyaç duyduğu tabloları Hangfire oluşturdu.

Hangfire’ın güzel özelliklerinden biri olan dashboard’ı görüyorsunuz buradan tüm job’larınızı inceleyip yönetebilmektesiniz.

Bizim oluşturduğumuz job da burada gözükmektedir. İstersek trigger ile job’umuzu tetikleyebiliriz.

Job’ımızı tetiklediğimizde çalışacaktır ve dashboard’da başarılılar kısmında bizim job’ımızın raporunu görüyoruz. Eğer bir hata fırlatsaydı hata mesajını da Hangfire raporlayacaktır.

Son olarak, Hangfire’in görev planlama ve iş sırası yönetimindeki değerini vurgulamak istiyorum. Hangfire, uygulama geliştiricilerinin işlerini otomatikleştirmelerini, performanslarını artırmalarını ve kullanıcı deneyimini iyileştirmelerini sağlayan güçlü bir araçtır.
