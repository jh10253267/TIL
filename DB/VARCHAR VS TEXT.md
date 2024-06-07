# VARCHAR VS TEXT

## 개요

다른 프로젝트의 DDL을 살펴보다가 조금 신기한 걸 발견했다.

지금까지는 RDB에서 텍스트를 저장할 때 CHAR 혹은 VARCHAR를 사용하는 것으로 알고있었다.

그러나 TEXT라는 자료형을 사용하는 것을 봤고 어떤 차이가 있는지 정리해보려한다.

일단 예약 서비스의 이벤트 정보로는 VARCHAR(4000)을 사용하고 있고 사용자의 리뷰 속성에는 TEXT를 사용하고 있었다.

## What is the Difference Between VARCHAR and TEXT in MySQL?

Type of Textual data can be stored in MySQL using TEXT or VARCHAR. 

When i took an reservation service project, I saw the column using TEXT type while other column "event info" using VARCHAR(4000)despite large amounts Strings.

So What's the difference? Which type should we use?

Significantly, VARCHAR type can store up to 65,535 bytes and we can limit the maximun size. But in case of TEXT type, we cannot fix limit size.

For example, if service should limit length of text filed, such as title limited length of 50, it doesnot need length more then 50. so you can use shortened column to save storage.

Due to these variations, one data type may be better suited for certain use case the the other.

Not only featural difference but performance. There are performance differneces between TEXT and VARCHAR in addition to differences in the volume of data type can contain VARCHAR typically performs better than TEXT. because it needs less storage and provides quicker access to the data.
While handling higher amounts of data, this spped benefit may be lost. Thus in MySQL, text vs varchar is an important topic. in






