# Design Patterns

## Design Challenges:

### Global Configuration Management
> Para este caso haremos uso del patron de diseño **Singleton** ya que sera el encargado de manejar una unica instancia de configuracion

```
public class Configuration {
  private static Configuration config;
  private static Properties properties = null;

  private Configuration() {
    input = this.getClass().getResourceAsStream("/application.properties");
    properties = new Properties();
    properties.load(input);
  }

  public static Configuration getInstance() {
    if (config == null) {
        config = new Configuration();
    }
    return config;
  }
}
```

### Dynamic Object Creation Based on User Input
> Para este caso usaremo el patron de **Factory** ya que crearemos varios elementos apartir de una interfaz comun

```
public class VehicleModel {
  public String color;
  public Integer capacityTank;
  public String type; 
}

public interface Vehicle {
    void startEngine();
    void fullUpGasTank();
}

public class Motorcycle implements Vehicle {
  @Override
  public void startEngine() {
    log.info("Se ha encendido la moto")
  }
  @Override
  public void fullUpGasTank(VehicleModel vehicle) {
    vehicle.capacityTank = 15;
  }
}

public class Car implements Vehicle {
  @Override
  public void startEngine() {
    log.info("Se ha encendido el carro")
  }
  @Override
  public void fullUpGasTank(VehicleModel vehicle) {
    vehicle.capacityTank = 35;
  }
}

public class Bus implements Vehicle {
  @Override
  public void startEngine() {
    log.info("Se ha encendido el autobus")
  }
  @Override
  public void fullUpGasTank(VehicleModel vehicle) {
    vehicle.capacityTank = 60;
  }
}

public class VehicleFactory {
  public Vechicle getVehicle(VehicleModel vehicleModel) {
    switch(vehicle.type) {
      case "car":
        return new Car();
      case "moto":
        return new Motorcycle();
      case "autobus":
        return new Bus();
    }   
  }
}

public class VechicleService {
  private VechileFactory factory;
  private static Integer PRICE_LITER = 9; 

  public void calculateTotalGas(VehicleModel vehicleModel){
    Vehicle vehicle = factory.getVehicle(vehicleModel);
    vehicle.startEngine();
    vehicle.fullUpGasTank(vehicleModel);
    log.info("El total para llenar el vehiculo es de {}", vehicle.capacityTank * PRICE_LITER);   
  }
}

```

### State Change Notification Across System Components
> Para este caso usaremos el patron de **Observer** ya que estaremos esperando un tipo de cambio para ser notificado

```
```

### Efficient Management of Asynchronous Operations
> Por utlimo ocuparemos el patron **Asynchronus** que nos ayudara a hacer operaciones asincronos para procesoso que pueden ejecutarse sin esperar una respuesta al cliente

```
```

