#  Access denied; you need (at least one of) the SUPER or SYSTEM_VARIABLES_ADMIN privilege(s) for this operation


## 해결

Full Text Index를 적용하고 생성된 인덱스를 확인하려 했지만 위와 같은 에러가 났다.

살펴보니 RDS에서 AWS에서 사용자에게 모든 권한을 다 주지 않아서 생기는 문제였다.

만약 SET GLOBAL을 이용해 수정하고 싶은 파라미터가 있다면 파라미터 그룹에 들어가서 변경해주어야한다.

직접 변경을 원하는 파라미터를 수정해줘서 해결.