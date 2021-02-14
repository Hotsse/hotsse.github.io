---
layout: post
title: Spring 국세청 홈택스 사업자 휴폐업 조회 API 개발
categories: [Spring]
comments: true
---

- pom.xml
{% highlight xml %}
...
<dependencies>
  <dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
  </dependency>
</dependencies>
...
{% endhighlight %}

- CallNtsTests.java
{% highlight java %}
package com.hotsse.sample;

import java.util.Map;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.web.client.RestTemplate;

import com.fasterxml.jackson.dataformat.xml.XmlMapper;

@SpringBootTest
class CallNtsTests {
	
	@Test
	void contextLoads() throws Exception {
		Map<String, Object> result = this.callNtsGetTaxType("2208717483");
		System.out.println("regYn = " + result.get("regYn"));
		System.out.println("validYn = " + result.get("validYn"));
	}
	
	@SuppressWarnings("unchecked")
	private Map<String, Object> callNtsGetTaxType(String socNo) {
		
		Map<String, Object> result = new HashMap<String, Object>();
		final String url = "https://teht.hometax.go.kr/wqAction.do?actionId=ATTABZAA001R08&screenId=UTEABAAA13&popupYn=false";
		
		try {
			String dongCode = socNo.substring(3, 5);
			String request = String.format("<map id=\"ATTABZAA001R08\"><pubcUserNo/><mobYn>N</mobYn><inqrTrgtClCd>1</inqrTrgtClCd><txprDscmNo>%s</txprDscmNo><dongCode>%s</dongCode><psbSearch>Y</psbSearch><map id=\"userReqInfoVO\"/></map>"
					, socNo
					, dongCode);
			
			String response = new RestTemplate().postForObject(url, request, String.class);
			Map<String, String> map = new XmlMapper().readValue(response, Map.class);
			
			if(map.get("smpcBmanTrtCntn")
					.trim()
					.equals("등록되어 있는 사업자등록번호 입니다.")) {
				result.put("regYn", "Y");
				
				if(Pattern.matches("부가가치세.*입니다\\."
						, map.get("trtCntn").trim())) {
					result.put("validYn", "Y");
				}
				else {
					result.put("validYn", "N");
				}
			}
			else {
				result.put("regYn", "N");
				result.put("validYn", "N");
			}
			
		}
		catch(Exception e) {
			e.printStackTrace();
			result.put("regYn", "N");
			result.put("validYn", "N");
		}
		
		return result;
	}
}

{% endhighlight %}
