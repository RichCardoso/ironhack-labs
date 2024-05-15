# Advanced Application of All SOLID Principles

## Refactorizar el siguiente codigo siguiendo los `SOLID Principles`
```
class SystemManager {
    processOrder(order) {
        if (order.type == "standard") {
            verifyInventory(order);
            processStandardPayment(order);
        } else if (order.type == "express") {
            verifyInventory(order);
            processExpressPayment(order, "highPriority");
        }
        updateOrderStatus(order, "processed");
        notifyCustomer(order);
    }
 
    verifyInventory(order) {
        // Checks inventory levels
        if (inventory < order.quantity) {
            throw new Error("Out of stock");
        }
    }
 
    processStandardPayment(order) {
        // Handles standard payment processing
        if (paymentService.process(order.amount)) {
            return true;
        } else {
            throw new Error("Payment failed");
        }
    }
 
    processExpressPayment(order, priority) {
        // Handles express payment processing
        if (expressPaymentService.process(order.amount, priority)) {
            return true;
        } else {
            throw new Error("Express payment failed");
        }
    }
 
    updateOrderStatus(order, status) {
        // Updates the order status in the database
        database.updateOrderStatus(order.id, status);
    }
 
    notifyCustomer(order) {
        // Sends an email notification to the customer
        emailService.sendEmail(order.customerEmail, "Your order has been processed.");
    }
}
```

## Refactor Code

> Se crean los modelos necesarios

```
public enum OrderType {
  STANDARD, EXPRESS
}

public class Order {
  public Long id;
  public OrderType type;
  public Long priority;
  public BigDecimmal amount;
  public Long quantity;
}
```

> Se identifica las violaciones `OCP, DIP, LSP`, se corrige usando interfaces funcionales y un factory

```
interface OrderProcessor {
  void process(Order order);
}

class ExpressOrderProcessor implements OrderProcessor {
  @Override
  public void process(Order order) {
    //Logica para procesar el pago express con su prioridad
  }
}

class StandardOrderProcessor implements OrderProcessor {
  @Override
  public void process(Order order) {
    //Logica para procesar el pago standard
  }
}

class ProcessorFactory {
  public static OrderProcessor getProcessor(OrderType type) {
    swtich(type) {
      case EXPRESS:
        return new ExpressOrderProcessor();
      case STANDARD:
        return new StandardOrderProcessor();
    }
  }
}
```

> Se identifica la violacion `SRP` y se corrige usando una capa exclusiva para el manejo de notificaciones

```
interface INotificatorService {
  void sendEmail(String email, String message);
}

class NotificatorService implements INotificatorService {
  @Override
  public void sendEmail(String email, String message) {
    //Logica de envio de email
  }
}
```

> Se identifica la violacion `SRP` y se corrige usando una capa de repository que maneja el acceso a la capa de datos

```
interface IOrderRepositoy {
  void updateStatus(Long id, Long status);
  boolean stockAvailable(Long id, Long quantity);
}

class OrderRepository implements IOrderRepository {
  private Connection connection;

  @Override
  public void updateStatus(Long id, Long status) {
    connection.execQuery("update orders set status = ?1 where id = ?2", id, status);
  }

  @Override
  public boolean stockAvailable(Long id, Long quantity) {
    Long stock = connection.execQuery("select stock from products where id = ?1", quantity);
    return stock >= quantity
  }
}
```

> Se crea el service que sera el encargado de manejar la logica de negocio de la orden

```
class OrderService {
  OrderRepository repository;

  public void processOrder(Order order) {
    
    validateStock(order);

    OrderProcessor processor = ProcessorFactory.getProcessor(order.type);
    processor.process(order);

    repository.updateStatus(order.id, 5);  //5 es order status completado
  }

  void validateStock(Order order) {
    if(!respository.stockAvailable(order.id, order.quantity) {
      throw new Error("Out of stock");
    }
  }
}
```
