### java处理跨域请求

## 全局配置

```
@Configuration
public class AllConfigurer implements WebMvcConfigurer {
	@Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
               .allowedOrigins("*")
                .allowedMethods("GET", "POST","DELETE")
                .allowCredentials(true).maxAge(3600);
    }
}

使用spring注解@Configuration，实现WebMvcConfigurer接口重写addCorsMappings，
然后进行相关配置。
```

## 单个配置

```
@CrossOrigin(origins = "*") 
@PostMapping("/reg")
public JsonResult<Void> reg(User user){
	userService.reg(user);
	return new JsonResult<>(SUCCESS,MESSAGE);
}

使用@CrossOrigin注解进行单个相关配置
```

## 后端不能接收request payload数据处理

>前端:

```
1.使用jq的ajax
2.原生ajax实例化FormData对象，传到后端
	var formData = new FormData();
	formData.append("username","意义");
	formData.append("password","123456");
	xhr.send(formData);
```

>后端:

```
private String getRequestPayload(HttpServletRequest req) { 
	StringBuilder sb = new StringBuilder(); 
	try(BufferedReader reader = req.getReader()) { 
		char[]buff = new char[1024]; 
		int len; 
		while((len = reader.read(buff)) != -1) { 
			sb.append(buff,0, len); 
		} 
	}catch (IOException e) { 
		e.printStackTrace(); 
	} 
	return sb.toString(); 
}
```
