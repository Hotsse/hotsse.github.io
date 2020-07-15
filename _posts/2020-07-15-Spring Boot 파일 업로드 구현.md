---
layout: post
title: Spring Boot 파일 업로드 구현
categories: [Spring]
comments: true
---

파일 업로드 순서

1. Client Request 로부터 MultipartFile 을 구한다.
2. FileSystem 에 원하는 경로에 파일이 이미 존재하는지 확인한다.
3. 경로가 존재하지 않는 경우 디렉토리를 생성한다.
4. 경로에 물리파일을 작성한다.
5. 물리파일에 임시파일인 MultipartFile 을 덮어쓴다.

-------------

MultipartFile

MultipartFile 는 Request 에서 multipart/form-data 의 형식으로 업로드 된 파일에 대한 정보를 담고 있다.  
이는, 임시파일 서버의 로컬 스토리지에 저장되어 있으며, FileSystem 을 통해 원하는 경로로 올바르게 옮겨주어야 한다.  

요청으로부터 MultipartFile 을 구하는 방식은 크게 두 가지가 있으며, 후술한다.

-------------

MultipartReuqest 를 활용한 업로드 파일 획득

{% highlight java %}
package com.nexon.quicksample.controller;

import java.io.File;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;
import org.springframework.web.multipart.MultipartRequest;

@RestController
@RequestMapping(value = "/file")
public class FileController {

	private String storagePath = "C:/storage/";
	
	@PostMapping(value = "/upload")
	public ResponseEntity<Map<String, Object>> upload(MultipartRequest mreq) {
		
		MultipartFile mf = mreq.getFile("uploadFile");
		final String fileNm = mf.getOriginalFilename();
		final long fileSize = mf.getSize();
		final String filePath = storagePath + fileNm;
		
		File file = new File(filePath);
		if(file.exists()) {
			// 이미 동일한 파일 존재
			return ResponseEntity.status(HttpStatus.CONFLICT).build();
		}
		
		// 주어진 경로에 존재하지 않는 모든 디렉토리 생성
		if(file.getParentFile().mkdirs()) {
			try {
				// 물리파일 생성
				file.createNewFile();
			}
			catch(IOException ie) {
				// 파일 생성 중 오류
				ie.printStackTrace();
				return ResponseEntity.status(HttpStatus.CONFLICT).build();
			}			
		}
		
		try {
			// 업로드 임시파일 -> 물리파일 덮어쓰기
			mf.transferTo(file);
		}
		catch(IOException ie) {
			// 덮어쓰기 중 오류
			ie.printStackTrace();
			return ResponseEntity.status(HttpStatus.CONFLICT).build();
		}
				
		Map<String, Object> fileInfo = new HashMap<String, Object>();
		fileInfo.put("filePath", filePath);
		fileInfo.put("fileNm", fileNm);
		fileInfo.put("fileSize", fileSize);
		
		return ResponseEntity.created(null).body(fileInfo);
	}	
}
{% endhighlight %}

기존 요청에 Multipart 정보가 추가된 MultipartRequest 를 획득하여 이에서 MultipartFile 을 파싱해내는 방식이다.  
레거시 시스템에서 많이 볼 수 있다.

-------------

@RequestPart 어노테이션을 활용한 업로드 파일 획득

{% highlight java %}
package com.nexon.quicksample.controller;

import java.io.File;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestPart;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

@RestController
@RequestMapping(value = "/file")
public class FileController {

	private String storagePath = "C:/storage/";
	
	@PostMapping(value = "/upload")
	public ResponseEntity<Map<String, Object>> upload(
			@RequestPart(name = "uploadFile", required = true) MultipartFile mf) {
		final String fileNm = mf.getOriginalFilename();
		final long fileSize = mf.getSize();
		final String filePath = storagePath + fileNm;
		
		File file = new File(filePath);
		if(file.exists()) {
			// 이미 동일한 파일 존재
			return ResponseEntity.status(HttpStatus.CONFLICT).build();
		}
		
		// 주어진 경로에 존재하지 않는 모든 디렉토리 생성
		if(file.getParentFile().mkdirs()) {
			try {
				// 물리파일 생성
				file.createNewFile();
			}
			catch(IOException ie) {
				// 파일 생성 중 오류
				ie.printStackTrace();
				return ResponseEntity.status(HttpStatus.CONFLICT).build();
			}			
		}
		
		try {
			// 업로드 임시파일 -> 물리파일 덮어쓰기
			mf.transferTo(file);
		}
		catch(IOException ie) {
			// 덮어쓰기 중 오류
			ie.printStackTrace();
			return ResponseEntity.status(HttpStatus.CONFLICT).build();
		}
				
		Map<String, Object> fileInfo = new HashMap<String, Object>();
		fileInfo.put("filePath", filePath);
		fileInfo.put("fileNm", fileNm);
		fileInfo.put("fileSize", fileSize);
		
		return ResponseEntity.created(null).body(fileInfo);
	}	
}

{% endhighlight %}

MultipartRequest 인터페이스를 사용하지 않고도 어노테이션을 통해 필요한 MultipartFile 을 얻어올 수 있다.  
다중 파일의 경우는 List&lt;MultipartFile&gt; 를 파라미터로 하여 List 의 형태로도 받을 수 있다.