# spring-boot-microservices

# Create a order-service with following code


## Entity class
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

## Repository Interface

```

package com.pratyush.orderservice.repository;

import com.pratyush.orderservice.entity.Order;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface OrderRepository extends JpaRepository<Order, Integer> {
}

```

## Service class

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

## Controller class

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

##create a file application.yml in resources package with following value

```

server:
  port:9191
  h2:
    console:
      enable:true
```
