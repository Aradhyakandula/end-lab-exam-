package com.klef.jfsd.exam;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;
import javax.persistence.criteria.CriteriaBuilder;
import javax.persistence.criteria.CriteriaQuery;
import javax.persistence.criteria.Predicate;
import javax.persistence.criteria.Root;
import javax.persistence.TypedQuery;
import java.util.List;

public class ClientDemo {
    public static void main(String[] args) {
        // Creating EntityManagerFactory and EntityManager
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("labexam");
        EntityManager em = emf.createEntityManager();

        // Inserting records manually
        em.getTransaction().begin();

        Customer customer1 = new Customer();
        customer1.setName("John Doe");
        customer1.setEmail("john.doe@example.com");
        customer1.setAge(30);
        customer1.setLocation("New York");
        em.persist(customer1);

        Customer customer2 = new Customer();
        customer2.setName("Jane Smith");
        customer2.setEmail("jane.smith@example.com");
        customer2.setAge(25);
        customer2.setLocation("Los Angeles");
        em.persist(customer2);

        Customer customer3 = new Customer();
        customer3.setName("Alice Johnson");
        customer3.setEmail("alice.johnson@example.com");
        customer3.setAge(35);
        customer3.setLocation("Chicago");
        em.persist(customer3);

        em.getTransaction().commit();

        // Criteria API operations (for example: age greater than 30)
        CriteriaBuilder cb = em.getCriteriaBuilder();
        CriteriaQuery<Customer> cq = cb.createQuery(Customer.class);
        Root<Customer> root = cq.from(Customer.class);

        // Apply condition where age is greater than 30
        Predicate condition = cb.gt(root.get("age"), 30);
        cq.where(condition);

        TypedQuery<Customer> query = em.createQuery(cq);
        List<Customer> results = query.getResultList();

        // Print results
        System.out.println("Customers with age greater than 30:");
        for (Customer customer : results) {
            System.out.println("ID: " + customer.getId() + ", Name: " + customer.getName() +
                    ", Email: " + customer.getEmail() + ", Age: " + customer.getAge() +
                    ", Location: " + customer.getLocation());
        }

        // Apply other conditions, for example: Name equals "John Doe"
        CriteriaQuery<Customer> cq2 = cb.createQuery(Customer.class);
        Root<Customer> root2 = cq2.from(Customer.class);
        Predicate condition2 = cb.equal(root2.get("name"), "John Doe");
        cq2.where(condition2);

        TypedQuery<Customer> query2 = em.createQuery(cq2);
        List<Customer> results2 = query2.getResultList();

        // Print results
        System.out.println("\nCustomers with name 'John Doe':");
        for (Customer customer : results2) {
            System.out.println("ID: " + customer.getId() + ", Name: " + customer.getName() +
                    ", Email: " + customer.getEmail() + ", Age: " + customer.getAge() +
                    ", Location: " + customer.getLocation());
        }

        // Close EntityManager
        em.close();
        emf.close();
    }
}
