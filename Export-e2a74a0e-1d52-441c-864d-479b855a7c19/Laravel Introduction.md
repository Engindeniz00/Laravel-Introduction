# Laravel

```bash
$ php artisan optimize
```

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled.png)

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%201.png)

```bash
php artisan make:controller HomeController

```

.env app uygulamaya ulaşmamıza sağlar

htaccess dosyaları yönlendirme dosyalarıdır

```bash
composer require --dev barryvdh/laravel-ide-helper
php artisan ide-helper:generate
```

kütüphaneler vendor dosyasında

template

indir dosyalar nerden çekiliyor onlara bak sonra kendi public dosyanın içine at

css js hepsini ayrı dosyaya tekrardan at

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%202.png)

HER ROUTE YAPTIĞINDA OPTIMIZE YAP!

comsoper yoksa vendor zaten olmuyor

config→app.php→provider içine → Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class, kütüphanesi yoksa eklenecek

```bash
--resource
route:list
```

orm yapıları sql uyumluluk sorunları hakkında çözüm bulabilmemizi sağlıyor.

database yapısını modeller üzerinde yapıyoruz.

## Laravel üzerinde veritabanı kontrolü

```bash
php artisan tinker
$urunler= DB::select('SELECT * FROM urun');
$urunler[0]
$urunler // direk bütün bilgileri gösterir
DB::insert("INSERT INTO urun (name,description,price) VALUES(?,?,?)",['elma','yenir','20']); // yeni bir item eklemek istediğimiz zaman
DB::update("UPDATE urun SET price=55 WHERE id=?",[2]);
=> 1 // etkilenen row sayısı
DB::table("urun")->insert(['name'=>'armut','price'=>44,'description'=>'kilosu pahalı']);
DB::table("urun")->insert([
            ['name'=>'soğan','price'=>83,'description'=>'kilosu normal'],
            ['name'=>'havuç','price'=>92,'description'=>'kilosu normal']

        ]);
```

> dd ve dump → dd ve dump fonkiyonu laravel için print gibi, dump ise içindekileri de ekrana basar ama dd aşağıdaki bilgileri hepsini sadece başlıklar halinde gösterir. dump çalışmayı durdurmaz dd ise çalışmayı durdurur
> 

birden fazla data için get ama sadece bir data için first kullanırız

- Update kullanımı

```bash
$urun=DB::table('urun')->where('id',9)->update(['price'=>97]);
```

> herhangi bir row etkilenirse site bize '1' gösterecektir
> 
- Delete kullanımı

```bash
$urun=DB::table('urun')->where('id',9)->delete();
```

### Laravel tinker direk olarak laravel üzerinden veri tabanı ile bağlantı kurup üzerinde işlem yapabilmemizi sağlar.

# Laravel ile model oluşturma

```bash
php artisan make:model Urun
```

Veri tabanı tablolarının aynısının class lara aktarılması. Artık sql üzerinden değil class'lar üzerinde verilerimi çekmiş olacağım. Her bir tabloya karşılık model vardır.

> Laravel urun → uruns diyer arar. Bunu engellemek için
> 

```bash
class Urun extends Model
{
    use HasFactory;
    protected $table="urun";
}
```

array olarak çekmek farklı obje olarak çekmek farklı. Model sistemini kullandığımız zaman obje olarak çekecektir.

### Datalar attribute içinde gözükür

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%203.png)

```php
public function getModelDb()
{
		$urunler = Urun::all();
		$urunler = Urun::where('price','>','50')->get();
		$urun= Urun::find(3);
		$urun = Urun::where('id',3)->first();
		dd($urun);
		dd($urunler);
}

```

modeller üzeirnde ekleme çıkarma yapmak için iki durum söz konusu bir tanesi fillable ve diğer guarded.  

```php
protected $fillable=['name','price'];
```

```php
protected $guarded=['name','price'];
```

timestamp bir date formatı anlık olarak tarihi gösteriyor.

migrate tabloların hepsini otomatik olarak mysql serverinin içine alıyor.

```php
php artisan migrate
// migrate etmeden önceki haline döndürüyor son yapılan değişikliğe geri alır
php artisan migrate:rollback
// reset ise direk veritabanını uçurur
php aritsan migrate:reset
// fresh önce uçurur sonra ise migrate ediyor.
php artisan migrate:fresh
```

categories olarak çoğul bir şekilde yazdığımız zaman laravel kendi sisteminde otomatik olarak categories şeklinde oluşturur. Fakat başka bir durumda create kullanmamız gerekir

```php
php artisan make:migration create_categories_table
php artisan make:migration create_categories_table --create=categories
```

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%204.png)

Model isimleri çoğul kullanılmaz!!.

Hem model oluşturup sonra migration ederk veritabanı ilişkisi sağlar

```php
php artisan make:model Category -m
```

### Slug nedir?

erkek-mont-kaban

### Tabloya yeni data kolonu eklemek için

```php
php artisan make:migration add_slug_to_categories_table --table=categories
Created Migration: 2021_10_24_063738_add_slug_to_categories_table
```

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%205.png)

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%206.png)

> Yukarıda tablo oluşturulur aşağıda ise tablo silinir.
> 

Burada bir yeni bir data eklediğimiz zaman varsayılan olarak is_active kısmına 1 olarak oluşturur.

```php
$table->enum('is_active',['0','1'])->default('1');
```

# Fake datalar ile çalışmak

seeder'lar.

```php
λ php artisan db:seed
Database seeding completed successfully.
```

> canlı ortama geçtiğim zaman bunlar kullanılır
> 

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%207.png)

> fakat geliştirme yaparken yani hazırlık aşamasında bunlar da kullanılır.
> 

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%208.png)

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%209.png)

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%2010.png)

php artisan db:seed

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%2011.png)

# Blade dosyalarına bilgi aktarımı gerçekten çok önemli!

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%2012.png)

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%2013.png)

![Untitled](Laravel%2072ccf51d16594006993a4685a95ffa24/Untitled%2014.png)

# Laravel Collection

[Collections](https://laravel.com/docs/8.x/collections)

## Console'da yazılan bütün kodlar

```php
php artisan db:seed
php artisan migrate:fresh
php artisan optimize
php artisan make:migration add_up_id_to_categories_table
php artisan config:clear
php artisan make:seeder CategorySeeder
php artisan migrate:rollback
php artisan make:migration add_slug_to_categories_table --table=categories
php artisan make:model Category -m
php artisan make:model Category
php artisan make:migration create_categories_table
php artisan make:migration create_categories_table --create=categories
php artisan tinker
php artisan make:controller Front\HomeController
```
