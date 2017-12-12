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
