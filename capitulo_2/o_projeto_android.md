## O Projeto Android {#o-projeto-android}

No aplicativo Android, a classe MainActivity típico deve ser derivado de uma classe chamada FormsApplicationActivity Xamarin.Forms definido no Xamarin.Forms.Platform.Android assemble, e a chamada Forms.Init requer algumas informações adicionais:

```java
using Android.App;
using Android.Content.PM;
using Android.OS;
namespace Hello.Droid
{
 [Activity(Label = "Hello", Icon = "@drawable/icon", MainLauncher = true,
 ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
 public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
 {
 protected override void OnCreate(Bundle bundle)
 {
 base.OnCreate(bundle);
 global::Xamarin.Forms.Forms.Init(this, bundle);
 LoadApplication(new App());
 }
 }
}
```

A nova instância da classe App é então passada para um método LoadApplication definido por FormsApplicationActivity. O atributo definido na classe MainActivity indica que a atividade não é recriada quando o telefone muda de orientação \(de retrato para paisagem ou para trás\) ou o tamanho da tela muda.

