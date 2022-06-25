# 0620

- **Transaction_Prestatement**
    - Transaction : 논리적인 작업단위
    - insert, delete,update 등의 작업들을 **하나의 논리적인 작업단위로 묶어서** 쿼리 실행시 모든 작업이 **정상 처리 된 경우**에는 **commit**을 실행해서 db에 반영하고
    - **하나라도 정상처리 되지 않는 경우**에는 **rollback**을 실행해서 작업단위내의 모든 작업을 취소한다
        - sql1
            - StringBuilder sql1 = new StringBuilder();
            sql1.append("insert into goodsinfo ");
            sql1.append("values(?, ?, ?, ?) ");
        - sql2
            - StringBuilder sql2 = new StringBuilder();
            sql2.append("update goodsinfo ");
            sql2.append("set price = ? ");
            sql2.append("where code = ? ");
        - 쿼리가 정상 실행될때만 commit
            - **if (result1 == 1 && result2 == 1) {**
                conn.**commit();**
                System.out.println("db에 반영됨...");
            } else {
                conn.rollback();
                System.out.println("db반영이 취소됨..."); // 2번 반복했을때 나옴
            }
