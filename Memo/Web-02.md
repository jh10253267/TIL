# 웹과 데이터베이스

## 개요
서버사이드 프로그래밍에서 데이터베이스의 도움 없이 서비스가 운영되는 경우는 거의 없다. 사용자의 요청을 받고 저장하고 사용자가 요청한 정보를 제공하는등 데이터베이스가 쓰인다. 웹에서 데이터베이스를 어떻게 조작하는지 알아보려한다.

## JDBC
 Java Database Connectivity는 웹에서 데이터베이스를 사용하기 위해 자바에서 제공하는 데이터베이스 API이다.
 인터페이스란 말은 구현체가 필요하다는 말인데 구현체는 각 데이터베이스 벤더사가 제공해주고 있다.
 JDBC는 데이터베이스와 자바프로그램 사이에서 네트워크 데이터를 처리하는 코드가 필요한데 JDBC드라이버가 이러한 역할을 수행한다.
 <br>
 JDBC를 이용한 프로그래밍 시나리오는 다음과 같다.
 * 드라이버를 로드한다.
 * Connection객체를 생성해 데이터베이스와 네트워크 연결을 맺는다.
 * Statemet 객체를 생성 및 질의 수행
 * 결과물이 있는 질의라면 ResultSet객체를 이용해 데이터를 받는다.
 * 모든 객체를 닫는다.
 ![이미지](https://cphinf.pstatic.net/mooc/20180201_49/1517475141729UGWfv_PNG/2_11_1_JDBC_.PNG?type=w760)

 여기서 사용한 객체를 닫아주는 이유는 데이터 베이스는 굉장히 많은 연결을 처리해야 한다. 그러나 각 작업이 종료되지 않으면 새로운 연결을 받을 수 없는 상황이 발생한다. close()메소드는 데이터베이스쪽에 연결을 끊어도 좋다는 신호를 주고 네트워크 연결을 종료한다.

 ``` java
 public List<GuestBookVO> getGuestBookList(){
		List<GuestBookVO> list = new ArrayList<>();
		GuestBookVO vo = null;
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try{
			conn = DBUtil.getConnection();
			String sql = "select * from guestbook";
			ps = conn.prepareStatement(sql);
			rs = ps.executeQuery();
			while(rs.next()){
				vo = new GuestBookVO();
				vo.setNo(rs.getInt(1));
				vo.setId(rs.getString(2));
				vo.setTitle(rs.getString(3));
				vo.setConetnt(rs.getString(4));
				vo.setRegDate(rs.getString(5));
				list.add(vo);
			}
		}catch(Exception e){
			e.printStackTrace();
		}finally {
			DBUtil.close(conn, ps, rs);
		}		
		return list;		
	}
 ```

 반드시 종료되어야한다라는 키워드를 들었을 때 자바에 익숙하다면 try~catch~finally구문을 떠올릴 것이다.<br> 이를 사용해도 좋고 try~with~resources라는 구문이 있는데 객체를 자동으로닫아주어 객체의 close가 보장된다.<br>Lombok에서는 @Cleanup이라는 애노테이션으로 간단하게 사용할 수 있다.<br>
 커넥션을 맺은 다음에 가장 중요한 객체는 Statement 혹은 PreparedStatement이다.<br>
 이 객체들은 sql을 실행할 수 있는 객체를 생성하는 기능인데 Connection객체로부터 얻어올 수 있다.<br><br>
 두 객체는 쿼리문을 전달한다는 점에선 같지만 쿼리문 내무의 모든 데이터를 같이 전송하는 방식과(Statement) 쿼리문을 미리 전달하고 나중에 데이터를 보내는 방식(PreparedStatement)이라는 점에서 다르다.<br><br>
  실제 개발에서는 PreparedStatement만을 사용하는 것이 관례인데 이는 SQL내부에서 고의적으로 다른 처리가 가능한 SQL injection을 막기 위함이다.

 ```java
 Connection conn = DriverManager.getConnection(url, user, password);
 PreparedStatment ps = conn.prepareStatement("쿼리문");
 ```
 주요 메소드는 setXXX()로 다양한 데이터 타입에 맞게 데이터를 세팅할 수 있다.
 executeUpdate() 쿼리문을 수행하고 결과를 int타입으로 반환한다.<br> 메소드의 이름에서 알 수 있듯이 insert,update,delete등 테이블에 영향을 미치는 쿼리문에 테이블의 몇 개의 행이 영향을 받았는지 알 수 있다.<br>
 executeQuery() select문을 실행할 때 사용한다.<br> executeQuery는 ResultSet을 리턴 타입으로 갖는다.<br> 커넥션과 마찬가지로 닫아줘야 데이터 베이스 내부에서 메모리와 같이 사용했던 자원들이 정리된다.<br>

 ResultSet이라는 인터페이스를 통해 자바 코드에서 데이터를 읽어들인다. 그래서 getXXX와 같이 필요한 타입으로 데이터를 받는다. 여기서 next()라는 메소드가 중요한데 데이터를 순차적으로 읽는 방식으로 작동하기 때문에 여려 row의 데이터가 있을 때 다음 행으로 이동하는 작업이 필요하다. 역시 네트워크를 통해서 데이터를 읽어들이기 때문에 작업이 끝난 후에는 반드시 close()를 사용해주어야 데이터베이스에서도 즉각적으로 정리할 수 있다.

## Connection Pool과 DataSource
  JDBC 프로그래밍은 필요한 순간에 잠깐 데이터베이스와 네트워크로 연결하고 데이터를 주고 받는 방식으로 구성되는데 이 과정에서 데이터베이스와 연결을 맺는 작업은 많은 시간과 자원을 쓰기 때문에 SQL문을 여러번 수행할수록 성능저하를 피할 수 없게 된다. 이를 위한 솔루션이 바로 <b>ConnectionPool</b>이다.

 ![이미지](https://cphinf.pstatic.net/mooc/20180208_14/15180684447693OANG_JPEG/3_8_2_ConnectionPool.jpg?type=w760)
커넥션 풀은 미리 커넥션을 여러개 맺어두고 커넥션이 플요하면 커넥션 풀에게 빌려서 사용한 후 반납한다.

데이터 소스는 커넥션풀을 관리하는 목적으로 사용된다. 커넥션을 얻고 반납하는 등의 작업을 수행한다. 위에서 살펴본 클로즈 메소드는 커넥션을 반납하도록 구현이 되어있다.<br> 인터페이스로 커넥션 풀을 이용하는 라이브러리들은 모두 데이터소스 인터페이스를 구현하고 있다. 커넥션 풀은 DBCP나 C3PO, HikariCP등이 있다.<br><br>
여기서 잠깐 생각을 해보자면 JDBC를 고전적으로 사용할 때 커넥션 생성할 때 데이터 베이스에 접속하기 위한 정보들을 넣어줬다. 그럼 커넥션풀과 데이터소스를 사용한다면 어디에 이런 정보를 넣어줘야할까? 커넥션 플을 관리하고 커넥션을 얻어오는 데이터 소스 객체에 넣어주는게 맞겠다.