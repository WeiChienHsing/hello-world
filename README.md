public Map<String, Object> getParams(HttpServletRequest request) {
		HashMap params = new HashMap();
		Set key = request.getParameterMap().keySet();
		Iterator it = key.iterator();

		while (it.hasNext()) {
			String name = (String) it.next();
			Object[] value = (Object[]) request.getParameterMap().get(name);
			if (value.length == 1) {
				if (value[0] != null && value[0].toString().length() > 0) {
					params.put(name, value[0]);
				}
			} else if (value.length > 1) {
				params.put(name, value);
			}
		}

		if (params.get("start") == null) {
			params.put("start", Integer.valueOf(0));
		} else {
			params.put("start",
					Integer.valueOf(params.get("start").toString().length() > 0
							? Integer.parseInt(params.get("start").toString())
							: 0));
		}

		if (params.get("limit") == null) {
			params.put("limit", Integer.valueOf(10));
		} else {
			params.put("limit",
					Integer.valueOf(params.get("limit").toString().length() > 0
							? Integer.parseInt(params.get("limit").toString())
							: 10));
		}

		params.put("session_user_ip", RequestUtil.getIpAddr(request));
		return params;
	}



@RequestMapping(value = "/pair") 
@ResponseBody 
public ResponseData pair(String rows, String group_name, String group_id, HttpServletRequest request) { 
User user = (User) SecurityContextUtil.getCurrentUser(); 
if (user == null) { 
user = (User) request.getSession().getAttribute(Constants.USER_OS); 
} 
Gson gson = new Gson(); 
List<SectionGroup> list = gson.fromJson(rows, new TypeToken<List<SectionGroup>>() {}.getType()); 
for (SectionGroup sectionGroup : list) { 
sectionGroup.setRegion(user.getRegion_id()); 
sectionGroup.setCompany_id(user.getOrg_id()); 
sectionGroup.setGroup_id(group_id); 
sectionGroup.setGroup_name(group_name); 
service.insertEntity(sectionGroup); 
} 
return ResponseData.SUCCESS_NO_DATA; 
} 
	
	
	
	var rows = $('#dg1').datagrid('getSelections'); 
$.ajax({ 
cache : false, 
type : "POST", 
url : _basePath + '/sectionGroup/pair', 
data : {rows : JSON.stringify(rows), group_id : group_id, group_name : group_name}, 
success : function(data) { 
if(data.success == true){ 
$.messager.confirm('配置成功','是否刷新列表？', function(r){ 
if (r){ 
$('#dg').datagrid('reload'); 
$('#dg1').datagrid('reload'); 
$('#dg2').datagrid('reload'); 
} 
}); 
}else{ 
$.messager.show({ 
title:'提示',msg:'配置失败', 
showType:'fade',style:{right:'',bottom:''} 
}); 
} 
} 
}); 
