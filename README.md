# xiang
work
import com.alibaba.druid.pool.DruidDataSourceFactory;
import xiang.Food;
import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;

public class jdbc {

    public static void main(String[] args)throws Exception {
        //注册驱动，版本高，可以省略

        //localhost:3306为默认，可以省略
        String url = "jdbc:mysql:///xiang?useSSL=false";
        String username = "root";
        String password = "123456";
        Connection conn = DriverManager.getConnection(url,username,password);

        Properties prop = new Properties();
        prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
        Connection connection = ((DataSource) dataSource).getConnection();


        //建表
        String sql1 = "create table if not exists tb_user (id int primary key auto_increment comment '号码''唯一身份认证''自增',username varchar(50) comment '用户名',email varchar(50) comment '邮箱',password varchar(50) comment '密码') comment '用户表'; ";
        String sql2 = "create table if not exists tb_food (id int primary key auto_increment comment '号码''唯一标识''自增',name varchar(50) comment '菜名',canteen varchar(50) comment '食堂',floor varchar(50) comment '楼层') comment '食物表'; ";
        String sql3 = "create table if not exists tb_Collection (food_id int comment '食物号码',user_id int comment '用户号码') comment '收藏表'; ";
        Statement stmt = conn.createStatement();
        int count1 = stmt.executeUpdate(sql1);
        if(count1==0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }
        int count2 = stmt.executeUpdate(sql2);
        if(count2==0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }
        int count3 = stmt.executeUpdate(sql3);
        if(count3==0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }

        //对用户信息进行基本CRUD
        String sql4 ="insert into tb_user (id,username,email,password)values(1,'王经纬','08221079@cumt.edu.cn','08221079')";
        String sql5 ="insert into tb_user (id,username,email,password)values(2,'Finch','**22****@cumt.edu.cn','**22****')";
        String sql6 ="insert into tb_user (id,username,email,password)values(3,'JiTiV','**22****@cumt.edu.cn','**22****')";
        int count4 = stmt.executeUpdate(sql4);
        if(count4>0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }
        int count5 = stmt.executeUpdate(sql5);
        if(count5>0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }
        int count6 = stmt.executeUpdate(sql6);
        if(count6>0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }
        String sql7 ="update User set name = '孙悟空' where id = 2";
        String sql8 ="delete from User where id=3";
        String sql9 ="select * from User";

        //食物表进行基本CRUE
        String sql10 ="insert into tb_food (id,name,canteen,floor)values(1,'重庆小面','一','1')";
        String sql11 ="insert into tb_food (id,name,canteen,floor)values(2,'三只羊烧烤','二','3')";
        String sql12 ="insert into tb_food (id,name,canteen,floor)values(3,'脆香鸡','三','1')";
        int count10 = stmt.executeUpdate(sql10);
        if(count10>0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }
        int count11 = stmt.executeUpdate(sql11);
        if(count11>0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }
        int count12 = stmt.executeUpdate(sql12);
        if(count12>0)
        {
            System.out.println("创建成功");
        }
        else
        {
            System.out.println("创建失败");
        }
        String sql13 ="update Food set name = '大碗面' where canteen = 3";
        String sql14="delete from Food where id=3";
        String sql15="select * from User";

        //新增收藏/取消收藏
        String sql16 ="insert into tb_collection (food_id ,user_id) value (1,1),(2,2),(3,3)";
        String sql17 ="delete from tb_collection where id = 2";

        //根据所属食堂和食堂的楼层进行筛选查询
        String sql19 = "select * from tb_food ";
        PreparedStatement pstmt = conn.prepareStatement(sql19);
        ResultSet rs = pstmt.executeQuery(sql19);
        //创建一个集合
        //光标向下一行，并且判断是否存在数据
        List<Food> foods = new ArrayList<>();
        Food food = new Food();
        while(rs.next()){
            //获取数据 getxxx()
            int id = rs.getInt("id");
            String name = rs.getString("name");
            String canteen = rs.getString("canteen");
            String floor = rs.getString("floor");

            food = new Food();
            food.setId(id);
            food.setName(name);
            food.setCanteen(canteen);
            food.setFloor(floor);

            //存入集合
            foods.add(food);
        }
        System.out.println(foods);

        //释放资源
        pstmt.close();
        rs.close();
        stmt.close();
        conn.close();
    }
}

