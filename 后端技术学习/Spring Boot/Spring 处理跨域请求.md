Spring设置Cors进行跨域请求
---

<!-- TOC -->

- [1. 处理跨域请求的第一种方式](#1-处理跨域请求的第一种方式)
- [2. 第二种跨域请求的方式](#2-第二种跨域请求的方式)

<!-- /TOC -->

# 1. 处理跨域请求的第一种方式
```java
public class CorsFilter extends OncePerRequestFilter {

    static final String ORIGIN = "Origin";

    protected void doFilterInternal(
        HttpServletRequest request, 
        HttpServletResponse response, 
        FilterChain filterChain) throws ServletException, IOException {
    
        String origin = request.getHeader(ORIGIN);
    
        response.setHeader("Access-Control-Allow-Origin", "*");//* or origin as u prefer
        response.setHeader("Access-Control-Allow-Credentials", "true");
        response.setHeader("Access-Control-Allow-Methods", "PUT, POST, GET, OPTIONS, DELETE");
        response.setHeader("Access-Control-Max-Age", "3600");
        response.setHeader("Access-Control-Allow-Headers", "content-type, authorization");
    
        if (request.getMethod().equals("OPTIONS"))
            response.setStatus(HttpServletResponse.SC_OK);
        else 
            filterChain.doFilter(request, response);
    }
}
```

# 2. 第二种跨域请求的方式
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;

/**
 * @Author stormbroken
 * Create by 2020/03/23
 * @Version 1.0
 **/

@Configuration
public class CORSConfig {
    private static String[] originsVal = new String[]{
            "localhost:8080",
            "127.0.0.1:8080",
            "127.0.0.1",
            "localhost",
            //"172.19.144.143",
            //"172.19.144.143:8000"
    };

    @Bean
    public CorsFilter corsFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        addAllowedOrigins(corsConfiguration); // 1 添加所有的允许访问的IP地址
        //corsConfiguration.addAllowedOrigin("*");
        corsConfiguration.addAllowedHeader("*"); // 2，如果限制POST等等则需要自行更改
        corsConfiguration.addAllowedMethod("*"); // 3
        corsConfiguration.setAllowCredentials(true); // 跨域session共享
        source.registerCorsConfiguration("/**", corsConfiguration); // 4
        return new CorsFilter(source);
    }

    private void addAllowedOrigins(CorsConfiguration corsConfiguration) {
        for (String origin : originsVal) {
            corsConfiguration.addAllowedOrigin("http://" + origin);
            corsConfiguration.addAllowedOrigin("https://" + origin);
        }
    }
}
```