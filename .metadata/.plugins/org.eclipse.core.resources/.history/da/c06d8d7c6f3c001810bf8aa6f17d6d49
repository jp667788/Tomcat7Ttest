package com.lijunpeng.my_tomcat;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.ServerSocket;
import java.net.Socket;

public class HttpServer {
	//判断是否需要关闭容器
	private boolean shutdown = false;
	
	public void acceptWait() {
		ServerSocket serverSocket = null ;
		
		try {
			//利用socket监听8080端口
			serverSocket = new ServerSocket(8090, 1, InetAddress.getByName("127.0.0.1"));
		} catch (IOException e) {
			e.printStackTrace();
			System.exit(1);
		}
		
		//循环等待客户端发送请求
		while (!shutdown) {
			try {
				Socket socket = serverSocket.accept();
				InputStream is = socket.getInputStream();
				OutputStream os = socket.getOutputStream();
				//接受请求参数，封装request参数
				Request request = new Request(is);
				request.parse();
				//创建用于返回浏览器的对象response
				Response response = new Response(os);
				response.setRequest(request);
				response.sendStaticResource();
				//关闭一次请求的socket，因为http请求就是采用短连接的方式
				socket.close();
				if (null != request && null != request.getUrl()) {
					shutdown = request.getUrl().equals("/shutdown");
				}
				
			} catch (IOException e) {
				e.printStackTrace();
				continue;
			}
		}
	}
	
	public static void main(String[] args) {
		HttpServer server = new HttpServer();
		server.acceptWait();
	}
	
}
