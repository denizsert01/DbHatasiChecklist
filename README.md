# DbHatasiChecklist

 DbContext ve Database Hatası Kontrol Listesi
1. DbContext Constructor'ı doğru mu?
public SirketDbContext(DbContextOptions<SirketDbContext> options) : base(options) { } ✅ Constructor var mı?
✅ İçinde DbContextOptions alıyor mu?

Yoksa: → Hemen ekle, çünkü EF Core Migration sırasında bu lazım.

2. Program.cs içinde DbContext doğru kayıtlı mı?
csharp
Kopyala
Düzenle
builder.Services.AddDbContext<SirketDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("ConnStr")));
✅ AddDbContext var mı?
✅ Doğru connection string ismi mi kullanılıyor?

Yanlışsa: → Connection string adıyla Program.cs eşleşmeli.

3. Identity doğru mu bağlı?
csharp
Kopyala
Düzenle
builder.Services.AddIdentity<AppUser, IdentityRole<int>>()
    .AddEntityFrameworkStores<SirketDbContext>();
✅ Identity ayarı doğru mu?
✅ .AddEntityFrameworkStores<SirketDbContext>() var mı?

Yoksa: → Login/Register çalışmaz ve Migration hata verir.

4. DbSet'ler Eksiksiz Tanımlandı mı?
SirketDbContext içinde:

csharp
Kopyala
Düzenle
public DbSet<Company> Companies { get; set; }
public DbSet<Department> Departments { get; set; }
public DbSet<CompanyDepartment> CompanyDepartments { get; set; }
✅ Tüm Entity'ler DbContext içinde DbSet olarak tanımlı mı?

Eksikse: → EF Core Migration'da Entity'yi bulamaz.

5. Entity'lerin Namespace ve İsimleri doğru mu?
using ProjeAdi.Entities; gibi doğru namespace import edildi mi?

Entity isimleri (Company, Department vs.) doğru mu?

Yanlışsa: → Tip bulunamaz hatası verir.

6. Configuration'lar doğru bağlı mı?
builder.ApplyConfigurationsFromAssembly(typeof(SirketDbContext).Assembly); ✅ OnModelCreating içinde bu var mı?

Yoksa: → CFG dosyaları okunmaz, HasData çalışmaz.

7. Connection String doğru mu yazıldı?
appsettings.json içinde doğru mu?

json
Kopyala
Düzenle
"ConnectionStrings": {
  "ConnStr": "Data Source=LAPTOP-XYZ\\SQLEXPRESS;Initial Catalog=SirketDB;Integrated Security=True;TrustServerCertificate=True"
}
✅ Server ismi doğru mu? ✅ Initial Catalog (veritabanı adı) doğru mu?

Yanlışsa: → Database connection hatası alırsın.

8. Migration Komutları Doğru Kullanılıyor mu?
Add-Migration InitialCreate

Update-Database

✅ Önce Migration, sonra Database güncellemesi yapılıyor mu?

Sırasız yapılırsa: → Mapping veya veri kaybı olur.

9. Çift Constructor var mı?
Sadece 1 constructor olmalı: DbContextOptions alan.

Veya: Hatalı boş constructor (protected SirketDbContext() {}) olmamalı.

10. Eski Build Dosyalarını Temizledin mi?
Bazen Visual Studio/Rider eski hatalı build dosyalarını tutar:

Clean Solution yap.

Sonra Rebuild Solution yap.

Yapmazsan: → Eski DLL'lerden hata gelebilir.
