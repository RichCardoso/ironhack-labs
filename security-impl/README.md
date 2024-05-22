# Scenarios for Security Design:
## Scenario 1: Pseudo-Code for Authentication System
> Example:
```
FUNCTION authenticateUser(username, password):
  QUERY database WITH username AND password
  IF found RETURN True
  ELSE RETURN False
```

> Solucion: Crear consultas parametrizadas para prevenir inyecciones SQL e implementar un sistema de reintentos
> fallidos para bloquear al usuario y prevenir un ataque en el logueo, adicional de hacer un hash en el password
> para no manejar datos sensible en claro en la base de datos
```
  public UserEntity searchUserbyNameAndPassword(String username, String password) {
        Query query = new Query();
        query.addCriteria(Criteria.where("username").is(username)
            .and("password").is(password);

        return query.execute();
  }
```

```
  public boolean authenticate(String username, String password) throws Exception {
        SessionEntity session = authenticateRepository.searchAttemptsLogin(user);
        if (session != null && session.getAttemptsLogin() >= 3) {
            log.error("User has reached the maximum number of login attempts");
            return throw new Exception("User has reached the maximum number of login attempts");
        }
        UserEntity user = authenticateRepository.searchUserbyNameAndPassword(username, hashPassword(password));

        if (user != null) {
            return true;
        } else {
            authenticateRepository.upsertSession(user);
            log.error("User not found (more details of error) ");
            return throw new Exception("User not found");

        }
  }
```

## Scenario 2: JWT Authentication Schema
> Example:
```
DEFINE FUNCTION generateJWT(userCredentials):
  IF validateCredentials(userCredentials):
    SET tokenExpiration = currentTime + 3600 // Token expires in one hour
    RETURN encrypt(userCredentials + tokenExpiration, secretKey)
  ELSE:
    RETURN error
```

> Solucion: Se mejora la funcion construyendo un JWT firmado mediante un secret key usando un
> algoritmo de encriptado y se crea una funcion para validar el JWT donde primero valida la expiracion
> y tambien verifica que la firma del JWT se congruente con el contenido del mismo,
> de esta forma aseguramos que el JWT creado para el usuario es valido para el consumo de los servicios

```
    public void generateJWT(UserCredential userCredential) {
        String tokenExpiration = currentTime + 3600; // Token expires in one hour
        String header = createHeader();
        String payload = createPayload(userCredential, tokenExpiration);
        String signature = createSignature(header, payload, secretKey); //Se firma el JWT usando un algoritmo de encriptacion como SHA256 para asegurar quen la peticion es valida

        String jwt = header + "." + payload + "." + signature;
    }

    public void validJWT(String jwt) {
        Integer expires = getExpiration(jwt);
        if (currentTime > expires) {
            throw new Exception("JWT expired");
        }

        return verifySignature(jwt);
    }
```

## Scenario 3: Secure Data Communication Plan
> Example: 
```
PLAN secureDataCommunication:
IMPLEMENT SSL/TLS for all data in transit
USE encrypted storage solutions for data at rest
ENSURE all data exchanges comply with HTTPS protocols
```

> Solucion: Se agrega el uso de certificados para la comunicacion segura y el redireccionamiento al uso de https
```
PLAN secureDataCommunication:
IMPLEMENT SSL/TLS for all data in transit
USE encrypted storage solutions for data at rest
ENSURE all data exchanges comply with HTTPS protocols
USE certificates from a trusted Certificate Authority (CA)
REDIRECT all HTTP traffic to HTTPS
```
