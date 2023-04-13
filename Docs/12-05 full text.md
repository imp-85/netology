mysql> EXPLAIN ANALYZE
    -> select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
    -> from payment p, rental r, customer c, inventory i, film f
    -> where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id;
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| EXPLAIN







                                                                           |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| -> Table scan on <temporary>  (cost=2.50..2.50 rows=0) (actual time=7123.288..7123.355 rows=391 loops=1)
    -> Temporary table with deduplication  (cost=0.00..0.00 rows=0) (actual time=7123.286..7123.286 rows=391 loops=1)  
        -> Window aggregate with buffering: sum(payment.amount) OVER (PARTITION BY c.customer_id,f.title )   (actual time=2956.705..6893.947 rows=642000 loops=1)
            -> Sort: c.customer_id, f.title  (actual time=2956.615..3071.696 rows=642000 loops=1)
                -> Stream results  (cost=10464117.80 rows=16086000) (actual time=0.511..2223.100 rows=642000 loops=1)  
                    -> Nested loop inner join  (cost=10464117.80 rows=16086000) (actual time=0.503..1875.402 rows=642000 loops=1)
                        -> Nested loop inner join  (cost=8851496.30 rows=16086000) (actual time=0.497..1595.894 rows=642000 loops=1)
                            -> Nested loop inner join  (cost=7238874.80 rows=16086000) (actual time=0.489..1313.008 rows=642000 loops=1)
                                -> Inner hash join (no condition)  (cost=1608774.80 rows=16086000) (actual time=0.473..73.746 rows=634000 loops=1)
                                    -> Filter: (cast(p.payment_date as date) = '2005-07-30')  (cost=1.68 rows=16086) (actual time=0.040..9.103 rows=634 loops=1)
                                        -> Table scan on p  (cost=1.68 rows=16086) (actual time=0.025..5.282 rows=16044 loops=1)
                                    -> Hash
                                        -> Covering index scan on f using idx_title  (cost=103.00 rows=1000) (actual time=0.044..0.313 rows=1000 loops=1)
                                -> Covering index lookup on r using rental_date (rental_date=p.payment_date)  (cost=0.25 rows=1) (actual time=0.001..0.002 rows=1 loops=634000)
                            -> Single-row index lookup on c using PRIMARY (customer_id=r.customer_id)  (cost=0.00 rows=1) (actual time=0.000..0.000 rows=1 loops=642000)
                        -> Single-row covering index lookup on i using PRIMARY (inventory_id=r.inventory_id)  (cost=0.00 rows=1) (actual time=0.000..0.000 rows=1 loops=642000)
 |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (7,13 sec)

mysql> SELECT DISTINCT 
    -> concat(c.last_name ,' ',c.first_name),
    -> sum(p.amount)
    -> FROM payment p
    -> LEFT JOIN customer c on c.customer_id =p.customer_id
    -> LEFT JOIN rental r on r.rental_date = p.payment_date 
    -> WHERE  p.payment_date >= '2005-07-30' and p.payment_date < DATE_ADD('2005-07-30', INTERVAL 1 DAY)
    -> GROUP BY p.customer_id;
+---------------------------------------+---------------+
| concat(c.last_name ,' ',c.first_name) | sum(p.amount) |
+---------------------------------------+---------------+
| ADAMS KATHLEEN                        |          7.98 |
| ALEXANDER DIANA                       |          8.98 |
| ALLARD GORDON                         |         12.98 |
| ALLEN SHIRLEY                         |          0.99 |
| ALVAREZ CHARLENE                      |         18.98 |
| ANDERSON LISA                         |         16.98 |
| ARCE HARRY                            |          5.99 |
| ARCHULETA JORDAN                      |          5.98 |
| ARTIS CARL                            |          7.98 |
| AUSTIN ALMA                           |          5.99 |
| BAILEY MILDRED                        |          4.99 |
| BANDA EVERETT                         |          0.99 |
| BANKS JESSIE                          |          2.99 |
| BARBEE CLAYTON                        |          3.98 |
| BARKLEY VICTOR                        |         21.97 |
| BARNETT CAROLE                        |          2.99 |
| BARRETT TRACEY                        |          1.98 |
| BATES DAISY                           |         10.98 |
| BAUGH EDWARD                          |          2.99 |
| BAUGHMAN ROBERT                       |          4.99 |
| BENNER CHESTER                        |          7.98 |
| BENNETT JANE                          |          4.99 |
| BERRY REGINA                          |         11.97 |
| BESS CHARLIE                          |          9.99 |
| BETANCOURT MIGUEL                     |         10.99 |
| BISHOP AGNES                          |          3.98 |
| BLAKELY DEREK                         |          4.99 |
| BOLIN MATHEW                          |          0.99 |
| BONE DON                              |          8.98 |
| BOUDREAU BOBBY                        |          5.98 |
| BOURQUE DERRICK                       |          6.99 |
| BOWENS CLIFFORD                       |         17.97 |
| BRADLEY ANA                           |         19.97 |
| BREAUX TED                            |          0.99 |
| BREWER VICKIE                         |          3.98 |
| BRINSON RUSSELL                       |          7.98 |
| BROOKS BEVERLY                        |          2.99 |
| BROTHERS CHRIS                        |          0.99 |
| BROWN ELIZABETH                       |          6.98 |
| BROWNLEE GUY                          |          8.99 |
| BRYANT PAULA                          |          6.99 |
| BULL WESLEY                           |          2.99 |
| BURKE LYDIA                           |          2.99 |
| BURLESON SIDNEY                       |          3.99 |
| BURNS APRIL                           |         16.97 |
| BURTON COLLEEN                        |          2.99 |
| BUSTAMANTE LEROY                      |          4.99 |
| BUTLER LOIS                           |          8.97 |
| CABRAL DANIEL                         |          7.99 |
| CAMPBELL CATHERINE                    |          8.99 |
| CARPENTER LORETTA                     |          6.98 |
| CARR EILEEN                           |          4.99 |
| CARROLL JUNE                          |          6.99 |
| CARTER AMANDA                         |          8.99 |
| CASILLAS ALFRED                       |          2.99 |
| CASTRO JENNY                          |          1.99 |
| CHAMBERS KRISTINA                     |          8.98 |
| CHAPMAN TONYA                         |          6.99 |
| CHAVEZ KRISTEN                        |          7.99 |
| CHESTNUT PEDRO                        |          7.99 |
| CHISHOLM JOHNNIE                      |          6.98 |
| CHOATE RAMON                          |         12.97 |
| CHRISTENSON NELSON                    |          7.99 |
| CHURCHILL FERNANDO                    |         11.98 |
| CINTRON AUSTIN                        |          6.99 |
| CLARK MICHELLE                        |          0.99 |
| CLARY ADRIAN                          |          4.99 |
| COLBY BERNARD                         |          7.98 |
| COLEMAN KATHRYN                       |         19.97 |
| COLLAZO TOMMY                         |          9.98 |
| COLLINS DIANE                         |          0.99 |
| CORNWELL BRETT                        |          0.99 |
| CRAIG BOBBIE                          |          2.99 |
| CROMWELL IVAN                         |         11.97 |
| CROUSE ALBERT                         |          0.99 |
| CRUZ KIM                              |          4.99 |
| CULP THEODORE                         |         14.97 |
| CUNNINGHAM STACY                      |          0.99 |
| DANIELS DANIELLE                      |          2.99 |
| DAVIDSON PATSY                        |          6.99 |
| DAY COURTNEY                          |          5.98 |
| DEAN MARCIA                           |          5.99 |
| DELUCA RON                            |         11.98 |
| DELVALLE WADE                         |          4.99 |
| DEVORE HERMAN                         |          4.99 |
| DIXON AMBER                           |          2.99 |
| DOUGLAS MARSHA                        |          6.99 |
| DUGGAN FREDDIE                        |          4.99 |
| DUNCAN SAMANTHA                       |          1.99 |
| EAST JEFF                             |         10.99 |
| EASTER BEN                            |          4.99 |
| EBERT LEO                             |          6.98 |
| EDWARDS JOYCE                         |          0.99 |
| EGGLESTON JIMMIE                      |          2.99 |
| ELLIOTT KATIE                         |          2.99 |
| ELROD JAVIER                          |          0.99 |
| ESTEP TRAVIS                          |          5.98 |
| EVANS ANN                             |          7.99 |
| FARNSWORTH JOHN                       |          3.98 |
| FENNELL ALEXANDER                     |          7.98 |
| FERGUSON BERTHA                       |          6.99 |
| FIELDS VICKI                          |          0.99 |
| FISHER CINDY                          |          6.99 |
| FLEMING MYRTLE                        |          4.99 |
| FLETCHER MAE                          |          2.99 |
| FLORES JULIA                          |          3.98 |
| FORD CRYSTAL                          |          7.97 |
| FORMAN MICHEAL                        |          0.99 |
| FORTNER HOWARD                        |          8.98 |
| FOWLER JO                             |          0.99 |
| FOX HOLLY                             |          9.97 |
| FRAZIER GLENDA                        |          3.99 |
| FULTZ GERALD                          |         10.98 |
| GAFFNEY FELIX                         |         12.96 |
| GAMEZ CLARENCE                        |          8.97 |
| GARCIA CAROL                          |          9.98 |
| GARDINER DAVE                         |          9.97 |
| GARRETT NELLIE                        |          4.99 |
| GARZA PEARL                           |          0.99 |
| GEARY RUBEN                           |          2.99 |
| GIBSON VICTORIA                       |          6.99 |
| GILBERT TANYA                         |          5.99 |
| GILLETTE DUSTIN                       |          2.99 |
| GILLILAND JOE                         |          1.99 |
| GILMAN DENNIS                         |          7.99 |
| GOMEZ JOSEPHINE                       |         14.97 |
| GONZALEZ MARTHA                       |          6.96 |
| GOOCH ADAM                            |          8.98 |
| GRAF DOUGLAS                          |          5.99 |
| GRAHAM RITA                           |          3.99 |
| GRANT MICHELE                         |          2.99 |
| GRAVES BRANDY                         |         16.96 |
| GRAY JUDY                             |          2.99 |
| GRECO CHRISTOPHER                     |          2.99 |
| GREEN VIRGINIA                        |          8.98 |
| GREENE JEANETTE                       |          4.99 |
| GREGORY SONIA                         |          2.99 |
| GRESHAM ALEX                          |         10.98 |
| GREY ROSS                             |         10.97 |
| GRIGSBY THOMAS                        |          3.98 |
| GRUBER ARMANDO                        |          5.99 |
| GUAJARDO HARVEY                       |          2.99 |
| GUILLEN ERIK                          |         11.97 |
| GUNDERSON TERRENCE                    |         11.96 |
| GUTIERREZ CARLA                       |         12.97 |
| HALE RAMONA                           |          8.98 |
| HALL JESSICA                          |          0.99 |
| HAMILTON GLADYS                       |          4.99 |
| HANNON SETH                           |          0.99 |
| HARDISON BRYAN                        |          5.99 |
| HARPER ROBERTA                        |          3.98 |
| HARRIS HELEN                          |          4.99 |
| HART DANA                             |          4.99 |
| HAUSER COREY                          |         11.98 |
| HAVENS ARNOLD                         |         29.95 |
| HAWKINS JILL                          |          4.98 |
| HAWKS LEE                             |          4.99 |
| HAYES ROBIN                           |         10.98 |
| HEATON SHAWN                          |          8.98 |
| HENDERSON ANDREA                      |          3.99 |
| HENRY PAULINE                         |          6.97 |
| HERNANDEZ ANGELA                      |          4.99 |
| HERRERA NORA                          |          5.99 |
| HERRMANN TRACY                        |          4.99 |
| HERZOG CLAUDE                         |          8.99 |
| HICKS MONICA                          |          3.99 |
| HILL ANNA                             |         11.97 |
| HITE ZACHARY                          |         14.98 |
| HOFFMAN MATTIE                        |          0.99 |
| HOLLAND MABEL                         |          2.99 |
| HOLMES LUCILLE                        |          2.99 |
| HOLT TONI                             |         11.98 |
| HORTON BILLIE                         |          7.98 |
| HOULE RAY                             |          2.99 |
| HOWARD ROSE                           |          3.99 |
| HUDSON LAUREN                         |          4.99 |
| HUEY BRANDON                          |          9.98 |
| HUNT ELEANOR                          |          8.98 |
| HUNTER CHARLOTTE                      |          0.99 |
| HURTADO JEREMY                        |          0.99 |
| IRBY CURTIS                           |          3.98 |
| JACKSON KAREN                         |         10.98 |
| JACOBS GEORGIA                        |          5.99 |
| JAMES KATHY                           |          0.99 |
| JENNINGS NAOMI                        |          4.99 |
| JENSEN LENA                           |         21.97 |
| JOHNSON PATRICIA                      |         30.95 |
| JONES BARBARA                         |         11.98 |
| JORDON JERRY                          |         14.98 |
| JUNG CHRISTIAN                        |         10.98 |
| KAHN ALAN                             |          4.99 |
| KELLEY ELSIE                          |          0.99 |
| KELLY DENISE                          |          4.99 |
| KENNEDY RHONDA                        |         20.96 |
| KINDER REGINALD                       |          4.98 |
| KING MELISSA                          |          2.99 |
| KNOTT KELLY                           |          4.99 |
| KOWALSKI CHARLES                      |          9.98 |
| LAMBERT MISTY                         |          8.99 |
| LANCE JACOB                           |          0.99 |
| LARSON HEIDI                          |          6.99 |
| LARUE DARYL                           |         12.96 |
| LAWRENCE LAURIE                       |          7.99 |
| LEE KIMBERLY                          |          4.99 |
| LEONE LOUIS                           |         14.97 |
| LEWIS SARAH                           |         15.98 |
| LINTON GEORGE                         |         10.98 |
| LOMBARDI DWIGHT                       |          5.99 |
| LONG JACQUELINE                       |          4.99 |
| LOPEZ AMY                             |          2.99 |
| LOVELACE BARRY                        |          3.99 |
| LOWE PRISCILLA                        |         11.97 |
| LUCAS VELMA                           |          6.99 |
| LYNCH JACKIE                          |          5.99 |
| MACKENZIE STEVE                       |          5.99 |
| MADRIGAL RALPH                        |         10.97 |
| MAHAN MATTHEW                         |          2.99 |
| MAHON DONALD                          |          3.99 |
| MARK JOSHUA                           |          9.98 |
| MARLOW SAMUEL                         |          0.99 |
| MARSHALL SHERRY                       |          2.99 |
| MARTEL CALVIN                         |         13.98 |
| MARTIN SANDRA                         |          0.99 |
| MARTINEZ RUTH                         |          6.98 |
| MARTINO HAROLD                        |          2.99 |
| MASON JUANITA                         |         14.98 |
| MATTHEWS ERICA                        |          0.99 |
| MAULDIN GREGORY                       |         11.97 |
| MAY GWENDOLYN                         |          2.99 |
| MCADAMS ALFREDO                       |         14.98 |
| MCALISTER RENE                        |         13.98 |
| MCCARTER MORRIS                       |         10.98 |
| MCCOY VERA                            |          6.99 |
| MCCRARY RICHARD                       |          4.99 |
| MCDUFFIE SAM                          |          4.99 |
| MCWHORTER RAYMOND                     |          7.99 |
| MEADOR RICARDO                        |          5.98 |
| MEEK ANTONIO                          |          8.98 |
| MENA CASEY                            |          9.98 |
| MEYER NATALIE                         |          0.99 |
| MILES BECKY                           |          6.99 |
| MILLARD SHANE                         |          5.99 |
| MILLS ALICIA                          |          5.99 |
| MILNER TOM                            |          8.98 |
| MITCHELL STEPHANIE                    |          5.99 |
| MOELLER RODNEY                        |          0.99 |
| MONTGOMERY STACEY                     |         10.98 |
| MOORE MARGARET                        |          4.99 |
| MORALES ANITA                         |          0.99 |
| MORGAN EVELYN                         |          3.97 |
| MORRELL CRAIG                         |          3.98 |
| MORRIS HEATHER                        |          4.99 |
| MORRISON BESSIE                       |         14.98 |
| MORRISSEY JASON                       |         14.97 |
| MOTLEY BRADLEY                        |         10.98 |
| MURPHY CHERYL                         |          8.99 |
| MURRELL MANUEL                        |         14.97 |
| NELSON DEBRA                          |          1.99 |
| NEUMANN RANDALL                       |          5.98 |
| NGO JUSTIN                            |          3.98 |
| NOE ELMER                             |          4.99 |
| NOLEN CODY                            |         15.98 |
| OCAMPO MARION                         |          7.98 |
| OGLESBY ISAAC                         |          5.98 |
| OLIVARES JORGE                        |         13.98 |
| ORTIZ SYLVIA                          |          6.99 |
| PAINE DAN                             |          8.99 |
| PALMER MEGAN                          |          4.99 |
| PARKER FRANCES                        |          4.99 |
| PATTERSON WANDA                       |         12.97 |
| PAYNE LYNN                            |          8.99 |
| PEARSON IRMA                          |          6.99 |
| PEREZ CAROLYN                         |          3.99 |
| PERKINS GERALDINE                     |          5.98 |
| PERRY SARA                            |          2.99 |
| PERRYMAN WALTER                       |          6.98 |
| PETERS SUE                            |          5.98 |
| PETERSON NICOLE                       |          3.99 |
| PFEIFFER BOB                          |          4.98 |
| PINSON JEFFERY                        |          2.99 |
| PITT MAX                              |          4.99 |
| POINDEXTER HECTOR                     |         14.97 |
| POULIN BILLY                          |         13.98 |
| POWELL ANNE                           |          7.99 |
| POWER DARRELL                         |          7.99 |
| PRICE IRENE                           |          3.98 |
| PURDY ANDREW                          |         15.98 |
| QUALLS STEPHEN                        |          7.98 |
| QUIGLEY TROY                          |          9.99 |
| QUINTANILLA ROGER                     |          2.99 |
| RAMOS EVA                             |          5.98 |
| RAY AUDREY                            |          4.99 |
| REED DORIS                            |          2.99 |
| REID CONSTANCE                        |         14.97 |
| REYES DEBBIE                          |          7.99 |
| REYNOLDS ROSA                         |         11.98 |
| RHOADS EDGAR                          |          5.98 |
| RHODES SHERRI                         |          2.99 |
| RICHARDS WILMA                        |          3.99 |
| RICHARDSON ASHLEY                     |          0.99 |
| RICO KEITH                            |          3.99 |
| RILEY BRITTANY                        |          2.99 |
| RINEHART MARK                         |         10.98 |
| ROBERT ERIC                           |         10.98 |
| ROBERTS CHRISTINE                     |          4.99 |
| ROBERTSON JOANNE                      |          2.99 |
| ROBINS GREG                           |          5.99 |
| ROBINSON SHARON                       |          8.98 |
| RODRIGUEZ LAURA                       |          0.99 |
| RODRIQUEZ VIOLET                      |          2.99 |
| ROGERS TERESA                         |          4.99 |
| ROSE DARLENE                          |          7.98 |
| ROSS MARILYN                          |          6.99 |
| ROUSH TERRANCE                        |          8.98 |
| RUIZ VIVIAN                           |          5.98 |
| RUNYON NATHAN                         |         12.94 |
| RYAN TARA                             |         10.99 |
| SALISBURY RYAN                        |          4.99 |
| SANBORN GENE                          |          0.99 |
| SANCHEZ JULIE                         |          4.99 |
| SANDERS TAMMY                         |         15.97 |
| SAUER DEAN                            |          2.99 |
| SCHMIDT ROSEMARY                      |          6.98 |
| SCHOFIELD LEONARD                     |          5.99 |
| SCHRADER JIMMY                        |          0.99 |
| SCHWARZ BRUCE                         |          7.98 |
| SCOTT REBECCA                         |          3.99 |
| SCROGGINS STANLEY                     |          9.98 |
| SELBY AARON                           |          2.99 |
| SHANKS EARL                           |          5.99 |
| SHAW CLARA                            |         17.96 |
| SHELBY RICKY                          |          5.98 |
| SIKES FRANCIS                         |          1.98 |
| SILVA MAXINE                          |          1.99 |
| SILVERMAN MICHAEL                     |         19.97 |
| SIMMONS TINA                          |          9.98 |
| SIMPSON ELLEN                         |          6.99 |
| SIMS VANESSA                          |          6.99 |
| SLEDGE GILBERT                        |         14.98 |
| SNYDER MARION                         |         12.98 |
| SOTO NINA                             |         10.98 |
| SPURLOCK KYLE                         |         14.96 |
| STEPHENS LORRAINE                     |          7.99 |
| STEWART ALICE                         |          0.99 |
| STONE VERONICA                        |          4.98 |
| SULLIVAN DAWN                         |         22.96 |
| SUTTON FELICIA                        |          7.98 |
| SWAFFORD PERRY                        |          8.99 |
| TALBERT GLEN                          |          0.99 |
| TEEL SALVADOR                         |          8.98 |
| TERRY JENNIE                          |          2.99 |
| THOMAS NANCY                          |         11.97 |
| THOMPSON DONNA                        |         24.95 |
| TOBIAS CLYDE                          |         17.96 |
| TOMLIN EDDIE                          |          4.99 |
| TROUTMAN FRANKLIN                     |          3.99 |
| TUCKER MARJORIE                       |         14.97 |
| VARGAS CHRISTY                        |          5.98 |
| VARNEY BENJAMIN                       |          4.99 |
| VASQUEZ TERRI                         |          2.99 |
| VINES CECIL                           |          7.98 |
| VU ROBERTO                            |         15.98 |
| WADE MARGIE                           |         15.97 |
| WAGGONER FRANK                        |          7.96 |
| WAGNER DOLORES                        |         10.98 |
| WALDROP HUGH                          |         13.98 |
| WALKER DEBORAH                        |         12.97 |
| WALLACE CONNIE                        |          2.99 |
| WARD JANICE                           |          5.99 |
| WARREN HAZEL                          |          1.99 |
| WASHINGTON RUBY                       |         12.98 |
| WATKINS YVONNE                        |          4.99 |
| WATSON THERESA                        |          8.98 |
| WATTS SHELLY                          |          2.99 |
| WAUGH JAMIE                           |          5.98 |
| WAY MIKE                              |         10.98 |
| WEBB ETHEL                            |         14.98 |
| WEINER RONALD                         |          5.98 |
| WELCH MARLENE                         |          5.99 |
| WEST EDNA                             |          0.99 |
| WHEAT FRED                            |          5.98 |
| WHEELER LUCY                          |          3.99 |
| WHITE BETTY                           |          4.99 |
| WILLIAMS LINDA                        |          5.98 |
| WILLIAMSON GINA                       |         14.97 |
| WILLIS BERNICE                        |         15.97 |
| WOFFORD VIRGIL                        |          4.98 |
| WOOD LORI                             |         10.98 |
| WOODS FLORENCE                        |          5.98 |
| WRIGHT BRENDA                         |          0.99 |
| YOUNG CYNTHIA                         |          2.99 |
+---------------------------------------+---------------+
391 rows in set (0,01 sec)

mysql> EXPLAIN ANALYZE SELECT DISTINCT  concat(c.last_name ,' ',c.first_name), sum(p.amount) FROM payment p LEFT JOIN customer c on c.customer_id =p.customer_id   LEFT JOIN rental r on r.rental_date = p.payment_date  WHERE  p.payment_date >= '2005
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| EXPLAIN







                                                                           |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| -> Sort with duplicate removal: `concat(c.last_name ,' ',c.first_name)`, `sum(p.amount)`  (actual time=10.448..10.483 rows=391 loops=1)
    -> Table scan on <temporary>  (actual time=10.193..10.252 rows=391 loops=1)
        -> Aggregate using temporary table  (actual time=10.192..10.192 rows=391 loops=1)
            -> Nested loop left join  (cost=2883.73 rows=1787) (actual time=0.072..9.658 rows=642 loops=1)
                -> Nested loop left join  (cost=2258.29 rows=1787) (actual time=0.064..8.414 rows=634 loops=1)
                    -> Filter: ((p.payment_date >= TIMESTAMP'2005-07-30 00:00:00') and (p.payment_date < <cache>(('2005-07-30' + interval 1 day))))  (cost=1632.85 rows=1787) (actual time=0.055..7.767 rows=634 loops=1)
                        -> Table scan on p  (cost=1632.85 rows=16086) (actual time=0.040..4.667 rows=16044 loops=1)    
                    -> Single-row index lookup on c using PRIMARY (customer_id=p.customer_id)  (cost=0.25 rows=1) (actual time=0.001..0.001 rows=1 loops=634)
                -> Covering index lookup on r using rental_date (rental_date=p.payment_date)  (cost=0.25 rows=1) (actual time=0.001..0.002 rows=1 loops=634)
 |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0,01 sec)

mysql>
