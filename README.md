# spring-boot-microservices

# Create a order-service with following code


#### Entity class
```

package com.pratyush.orderservice.entity;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="ORDER_TB")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Order {

    @Id
    private int id;

    private String name;

    private int quantity;

    private double price;
}

```

#### Repository Interface

```

package com.pratyush.orderservice.repository;

import com.pratyush.orderservice.entity.Order;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface OrderRepository extends JpaRepository<Order, Integer> {
}

```

#### Service class

```

package com.pratyush.orderservice.service;

import com.pratyush.orderservice.entity.Order;
import com.pratyush.orderservice.repository.OrderRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class OrderService {

    @Autowired
    private OrderRepository repository;

    public Order saveOrder(Order order){
        return repository.save(order);
    }
}

```

#### Controller class

```

package com.pratyush.orderservice.controller;

import com.pratyush.orderservice.entity.Order;
import com.pratyush.orderservice.service.OrderService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/order")
public class OrderController {

    @Autowired
    private OrderService service;

    @PostMapping("/bookOrder")
    public Order bookOrder(@RequestBody Order order){
        return service.saveOrder(order);
    }
}
```

#### create a file application.yml in resources package with following value

```

server:
  port : 9192
spring:
  h2:
    console:
      enabled: true
```


# Here, We will write code for payment-service

#### Entity class

```
package com.pratyush.paymentservice.entity;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="PAYMENT_TB")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment {

    @Id
    @GeneratedValue
    private int paymentId;

    private String paymentStatus;

    private String transactionId;
}

```

#### Repository class

```
package com.pratyush.paymentservice.repository;

import com.pratyush.paymentservice.entity.Payment;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface PaymentRepository extends JpaRepository<Payment, Integer> {
}

```


#### Service class

```

package com.pratyush.paymentservice.service;

import com.pratyush.paymentservice.entity.Payment;
import com.pratyush.paymentservice.repository.PaymentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.UUID;

@Service
public class PaymentService {

    @Autowired
    private PaymentRepository repository;

    public Payment doPayment(Payment payment){

        payment.setTransactionId(UUID.randomUUID().toString());
        return repository.save(payment);
    }
}

```

#### Controller class
```

package com.pratyush.paymentservice.controller;

import com.pratyush.paymentservice.entity.Payment;
import com.pratyush.paymentservice.service.PaymentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/payment")
public class PaymentController {

    @Autowired
    private PaymentService service;

    @PostMapping("/doPayment")
    public Payment doPayment(@RequestBody Payment payment){
        return service.doPayment(payment);
    }
}

```

#### create a file application.yml in resources package with following value

```

server:
  port : 9191
spring:
  h2:
    console:
      enabled: true

### Now it's time to see if order service is working or not. For this do the following things

1. Do the POST request on POSTMAN with url http://localhost:9192/order/bookOrder  and body

```
{
	"id":103,
	"name":"Watch",
	"quantity":1,
	"price":5400
}

```

2. Open http://localhost:9192/h2-console on any browser with JDBC and open your table

```
jdbc:h2:mem:testdb

```

### Now do the above thing for Payment also.

1. Do the POST request on POSTMAN with url http://localhost:9191/payment/doPayment  and body

```
{
	"paymentStatus":"Success"
}

```

2. Open http://localhost:9191/console on any browser with JDBC and open your table

```
jdbc:h2:mem:testdb

```
