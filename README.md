# mock
package com.capgemini.takehome.ui;
import java.util.*;

import com.capgemini.takehome.bean.Product;
import com.capgemini.takehome.service.InValidCodeException;
import com.capgemini.takehome.service.ProductService;
public class Client {
	public static void main(String args[]) throws InValidCodeException {
		Scanner sc=new Scanner(System.in);
		ProductService ps=new ProductService();
		while(true) {
			System.out.println("\n 1.Generate bill by entering product code and quantity \n 2.Exit");
			//System.out.println("Enter your choice");
			int ch=sc.nextInt();
			switch(ch) {
			case 1:
				System.out.println("enter product code");
				int productCode=sc.nextInt();
				try {
					boolean isValid=ps.validateCode(productCode);
				}
				catch(InValidCodeException ie) {
					System.out.println("please enter 4 digit valid code");
				}
				System.out.println("enter product quantity");
				int quantity=sc.nextInt();
				if(quantity<0) {
					System.out.println("quantity shuld not be zero");
				}
				else {
					Product p1=ps.getProductDetails(productCode);
					if(p1==null) {
						System.out.println("Sorry! the product code<<"+productCode+">> is not avaliable");
						break;
					}
					float total=p1.getProductprice()*quantity;
					System.out.println("product name"+"  "+p1.getProductname());
					System.out.println("product category"+" "+p1.getProductcategory());
                    System.out.println("product price(rs)"+" "+p1.getProductprice());
                    System.out.println("product quantity"+" "+quantity);
                    System.out.println("line in total(rs)"+" "+total);
                    break;
				}
                    case 2:
                    	System.exit(0);
				}

			}
		}
			
		}
	

package com.capgemini.takehome.util;
import java.util.HashMap;
import java.util.Map;

import com.capgemini.takehome.bean.Product;
public class CollectionUtil {
	public static Map<Integer,Product> products=new HashMap<Integer,Product>();
	static {
		products.put(1001, new Product(1001,"iphone","Electronics",35000));
		products.put(1002, new Product(1002,"LEDTV","Electronics",45000));
		products.put(1003, new Product(1003,"Teddy","Toys",800));
		products.put(1004, new Product(1004,"Telescope","Toys",5000));
	}
	public static Map<Integer,Product> getObject() {
		return products;
	}

}


package com.capgemini.takehome.bean;

public class Product {
	private int productid;
	private String productname;
	private String productcategory;
	private int productprice;
	
	public Product (int productid,String productname,String productcategory,int productprice) {
		super();
		this.productid=productid;
		this.productname=productname;
		this.productcategory=productcategory;
		this.productprice=productprice;
	}
	public int getProductid() {
		return productid;
	}
	public void setProductid(int productid) {
		this.productid = productid;
	}
	public String getProductname() {
		return productname;
	}
	public void setProductname(String productname) {
		this.productname = productname;
	}
	public String getProductcategory() {
		return productcategory;
	}
	public void setProductcategory(String productcategory) {
		this.productcategory = productcategory;
	}
	public float getProductprice() {
		return productprice;
	}
	public void setProductprice(int productprice) {
		this.productprice = productprice;
	}
	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return "productId=" +productid+ ",productName=" +productname+ ",productCategory=" +productcategory+",productprice=" +productprice ;
	}
	
	
	

}


package com.capgemini.takehome.service;

public class InValidCodeException extends Exception {

	public InValidCodeException(int productcode) {
		// TODO Auto-generated constructor stub
	}

}


package com.capgemini.takehome.service;
import java.lang.*;
import java.util.regex.Pattern;

import com.capgemini.takehome.bean.Product;
import com.capgemini.takehome.dao.ProductDAO;



interface IProductService {
	Product getProductDetails(int productcode);
	//public boolean validateCode(int productcode);
}
public class ProductService implements IProductService {
	ProductDAO pdao=new ProductDAO();

	@Override
	public Product getProductDetails(int productcode) {
		// TODO Auto-generated method stub
		Product details=pdao.getProductDetails(productcode);
		return details;
	}
	public boolean validateCode(int productcode) throws InValidCodeException {
		String code=Integer.toString(productcode);
		boolean res=Pattern.matches("[0-9]{4}",code);
		if(res==false)
				throw new InValidCodeException(productcode);
		return false;
	}

}



package com.capgemini.takehome.dao;

import java.util.HashMap;

import com.capgemini.takehome.bean.Product;
import com.capgemini.takehome.util.CollectionUtil;

interface IProductDAO {
	Product getProductDetails(int productCode);
}
public class ProductDAO implements IProductDAO  {
    HashMap<Integer,Product> hm=null;
    public ProductDAO() {
    	hm=new HashMap<Integer,Product>();
    	hm=(HashMap<Integer, Product>) CollectionUtil.getObject();
    }
	@Override
	public Product getProductDetails(int productcode) {
		// TODO Auto-generated method stub
		Product pro=hm.get(productcode);
		hm.put(productcode,pro);
		return pro;
	}

}


