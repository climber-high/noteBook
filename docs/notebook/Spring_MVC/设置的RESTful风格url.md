### 设置的RESTful风格url

>@PathVariable()注解

```
@RestController
@RequestMapping("/addresses")
public class AddressController extends BaseController {
	@Autowired
	private IAddressService addressService;
	
  //@RequestMapping注解设置{}占位符aid，在方法参数列表中添加@PathVariable注解，@PathVariable注解参数跟占位符aid对应，
    用来接收@RequestMapping占位符aid的值进行参数传递
  
	@RequestMapping("{aid}/set_default")
	public JsonResult<Void> setDefault(@PathVariable("aid") Integer aid ,HttpSession session){
		Integer uid = getUidFromSession(session);
		String username = getUsernameFromSession(session);
		addressService.setDefault(aid, uid, username);
		return new JsonResult<>(SUCCESS);
	}
}
```
