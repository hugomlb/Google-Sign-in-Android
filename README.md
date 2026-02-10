# Google Sign-In en Android: El caos de los SHAs

## ¿Por qué estoy escribiendo esto?

Cuando arranqué a hacer apps en **Firebase con Google Sign-In**, me encontré con las configuraciones de los SHA. Al principio funcionaba en desarrollo, pero en producción fallaba porque había otros códigos y entender para qué servía cada uno era un quilombo. Después de chocarme contra un par de paredes, lo terminé solucionando, anoté mis descubrimientos y seguí con mi vida.

Hace poco volví a tener que configurar Google Sign-In para un proyecto personal y me encontré con los mismos problemas. Por suerte, tenía anotada la solución, así que el tiempo para resolverlos fue considerablemente menor (y menos doloroso). Sin embargo, esas notas eran más _bullet points_ que un razonamiento del cómo y por qué funciona así. Este artículo busca darle un poco más de contexto a mis notas iniciales.

## El Stack

Como mencioné antes, las configuraciones de Google Sign-In que hice fueron con *Firebase*, pero lo escrito aquí es lo suficientemente general como para funcionar con cualquier otra plataforma donde lo quieras configurar.

## Un montón de SHAs

Todos tenemos un nombre (yo soy Hugo, vos sos LectorRecopado) e incluso tu app tiene un nombre formal (sería `com.empresafantabulosa.appfantabulosa`, aunque los pibes la llamamos "App Fantabulosa"). Eventualmente, todos tenemos también una firma. Los **SHAs** son como las firmas digitales de la app.

¿Entonces, por qué hay tantos SHAs? En este caso, provienen de tres fuentes:

- **`debug.keystore`**: Es la firma que se usa en desarrollo. Es similar a cuando en el supermercado te piden que firmes el ticket: podés hacer cualquier zarlanga y va a estar bien. La única condición es que el SHA de tu `debug.keystore` local coincida con el que registraste en los servicios de Google.
    
- **`release.keystore`**: Esta se usa en ambientes pre-productivos (como una APK de prueba) o para enviarle la app firmada a la Play Store. Es como un trámite bancario: acá la firma tiene que ser la real porque la van a revisar. Para la Play Store, tu "firma real" se define con la primera versión que subas.
    
- **Firma de Play Store**: Hasta aquí todo bien, pero ¿de dónde sale esta tercera firma? El proceso es así: vos le entregás tu app firmada a "John Google" (el que revisa la app... supongo). John verifica que tu firma sea la indicada, la archiva y genera un certificado nuevo que dice: _"Esta es la App Fantabulosa de Empresa Fantabulosa. Yo, John Google, lo certifico"_. Él firma eso y lo manda a la tienda.
    

> **El secreto:** En producción, lo que el sistema busca es que la firma que pone la Play Store coincida con alguna de las que tenés registradas en tu consola de Firebase o Google Cloud.

## ¿Dónde encuentro la firma de la Play Store?

Para que el login de Google funcione en la app que bajás del store, tenés que copiar el SHA desde la consola de desarrollador de Google Play e ir a:

**Probar y publicar → Integridad de la app → Firma de aplicaciones de Google Play → Ajustes**

Ese código SHA-1 que encontrás ahí es el que tenés que pegar en tu proyecto de Firebase.

# En Conclusión

Entender esas diferencias te ahorrarán muchos dolores de cabeza y, si falla el Google Sign in en producción es probablemente porque todavía tu *App Fantabulosa* no conoce a John Google. 
