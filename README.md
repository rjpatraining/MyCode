# MyCode
https://www.youtube.com/watch?v=KYNR5js2cXE&list=PLZV0a2jwt22s5NCKOwSmHVagoDW8nflaC&index=11

package com.rjpard.springsecurityjwtrd.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception{
        return httpSecurity
                .csrf(csrf -> csrf.disable())
                .authorizeHttpRequests(auth -> {
                    auth.anyRequest().authenticated();
                })
                .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
                .httpBasic(Customizer.withDefaults())
                .build();
    }

    @Bean
    public InMemoryUserDetailsManager user(){

        return new InMemoryUserDetailsManager(
                User.withUsername("raj")
                        .password("{noop}password")
                        .authorities("read")
                        .build()
        );
    }
}




package com.rjpard.springsecurityjwtrd.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.security.Principal;

@RestController
public class HomeController {

    @GetMapping
    public String home(Principal principal){
        return principal.getName();
    }
}


<?xml version="1.0" encoding="UTF-8"?>
<project xmlns=http://maven.apache.org/POM/4.0.0 xmlns:xsi=http://www.w3.org/2001/XMLSchema-instance
   xsi:schemaLocation=http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd>
   <modelVersion>4.0.0</modelVersion>
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>3.2.4</version>
      <relativePath/> <!-- lookup parent from repository -->
   </parent>
   <groupId>com.rjpard</groupId>
   <artifactId>springsecurityjwtrd</artifactId>
   <version>0.0.1-SNAPSHOT</version>
   <name>springsecurityjwtrd</name>
   <description>Demo project for Spring Boot</description>
   <properties>
      <java.version>17</java.version>
   </properties>
   <dependencies>
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
      </dependency>
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
      </dependency>

      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-devtools</artifactId>
         <scope>runtime</scope>
         <optional>true</optional>
      </dependency>
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-configuration-processor</artifactId>
         <optional>true</optional>
      </dependency>
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-test</artifactId>
         <scope>test</scope>
      </dependency>
   </dependencies>

   <build>
      <plugins>
         <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
         </plugin>
      </plugins>
   </build>

</project>



(base) Rajmohans-MBP:certs raj$ openssl genrsa -out keypair.pem 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
..............................+++++
.....................................+++++
e is 65537 (0x010001)
(base) Rajmohans-MBP:certs raj$ openssl rsa -in keypair.pem -pubout -out public.pem
writing RSA key
(base) Rajmohans-MBP:certs raj$ openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in keypair.pem -out private.pem

