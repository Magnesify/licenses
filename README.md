# Lisans Sisteminin Eklentiye Entegrasyonu

## 1. Verilen maven kütüphanesini pom.xml dosyanıza ekleyiniz.
Endpoint`den aldığınız JSON verisinin okunması gerekir. Bu nedenle replace vb. methodlar kullanmadan jackson-databind ile okuma işlemini gerçekleştiriyoruz. 

```XML
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.14.0</version>
        </dependency>
```

## 2. Aşşağıda verilen kod ile birlikte bir class oluşturunuz.
Bu kodun en temel amacı eklentinize eklediğiniz kütüphane olan jackson-databind`ı kullanarak endpointden gelen veriyi düzenlemeyi amaçlar. 

```JAVA
    static ObjectMapper objectMapper = new ObjectMapper();

    public static JsonNode get() {
        try {
            return objectMapper.readTree(getResponse("mail", "anahtar"));
        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }
    }

    public static String server() {
        return get().path("server").asText();
    }

    public static String key() {
        return get().path("key").asText();
    }

    public static String product() {
        return get().path("product").asText();
    }

    public static String email() {
        return get().path("email").asText();
    }

    public static void main(String[] args) {

        System.out.println(email());
        System.out.println(product());
        System.out.println(server());
        System.out.println(key());
    }
```

Bu kod içerisinde görülen email, key vb. parametreler ile birlikte lisans sistemini kullanabilirsiniz. getResponse() kullandığınız JDK`ya göre değişiklik gösterecektir.

Ardından **onEnable() - onLoad()** gibi Override methodlara yukarıda belirtilen **key(),product(),server(),email()** gibi fonksiyonları ekleyebilirsiniz.
İşte bir örneği.

```JAVA
    public void CheckLicense() {
        if(product().equalsIgnoreCase("MagnesifyDungeons")) {
            if(server().equalsIgnoreCase("server-ip")) {
                System.out.println("Lisans geçerli");
            } else {
                System.out.println("Lisans geçersiz");
                System.exit(0);
            }
            System.out.println("Lisans geçersiz");
            System.exit(0);
        }
    }
```

