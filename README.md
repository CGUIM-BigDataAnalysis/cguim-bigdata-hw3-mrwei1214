長庚大學 大數據分析方法 作業三
================

作業說明 （繳交時請直接刪除這個章節）
-------------------------------------

作業目的：練習初級爬蟲，並將爬蟲結果整理成資料框data.frame

依下列指示，完成網站內文分析：

-   爬取指定網站內容

    -   學號結尾 1,5,9:[Ptt NBA 版](https://www.ptt.cc/bbs/NBA/index.html)
-   試著爬出**至少100篇**文章（`30pt`）的**標題**、**推文數**與**作者ID**（各`10pt`）
    -   資料框欄位名稱：
        -   **標題**：Title
        -   **推文數**：PushNum
        -   **作者ID**：Author
    -   一頁只有20篇，該怎麼辦？
        -   提示：使用for + rbind()將分批爬取出的資料結合
        -   範例：dataframeAll&lt;-rbind(dataframe1,dataframe2)
        -   參考：[6.6 資料組合](http://yijutseng.github.io/DataScienceRBook/manipulation.html#section-6.6)
-   將爬取出的資料輸出至Markdown報告中（`10pt`）
    -   使用knitr::kable(資料框物件)整理輸出
-   用文字搭配程式碼解釋爬蟲結果
    -   共爬出幾篇文章標題？（程式碼與文字解釋各`5pt`）
        -   dim(), nrow(), str()皆可
    -   哪個作者文章數最多？（程式碼與文字解釋各`5pt`）
        -   table()
    -   其他爬蟲結果解釋（`10pt`）
        -   試著找出有趣的現象，不一定要用程式碼搭配解釋，也可只用文字

網站資料爬取
------------

``` r
library(rvest) ##第一次使用要先安裝 install.packages("rvest")
```

    ## Warning: package 'rvest' was built under R version 3.2.5

    ## Loading required package: xml2

    ## Warning: package 'xml2' was built under R version 3.2.5

``` r
##read_html
##html_nodes
##html_text
NBA_posts_list <- list()
for(i in 4580:4584){
NBAurl<-paste("https://www.ptt.cc/bbs/NBA/index",i,".html",sep = '')
NBAContent<-read_html(NBAurl)

post_Title <- NBAContent %>%
html_nodes(".title") %>% 
html_text()

post_PushNum <-NBAContent %>%
html_nodes(".nrec")%>%
html_text()

post_Author<-NBAContent%>%
html_nodes(".author")%>%
html_text()

data<-data.frame(title=post_Title,PushNum=post_PushNum,Author=post_Author)
NBA_posts_list[[i]] <- data
}
NBA_dataframe<- rbind(NBA_posts_list[[4580]],NBA_posts_list[[4581]],NBA_posts_list[[4582]],NBA_posts_list[[4583]],NBA_posts_list[[4584]])
```

爬蟲結果呈現
------------

``` r
knitr::kable(NBA_dataframe)
```

| title                                               | PushNum | Author       |
|:----------------------------------------------------|:--------|:-------------|
| \[情報\] Rookie Ladder Week 19                      | 8       | Vedan1213    |
| \[情報\] 巴克利說球爸打不贏他                       | 46      | hitman0527   |
| \[討論\] 姆斯守的住喬丹嗎？                         | X2      | kyle5241     |
| \[討論\] HoopsHype 將11梯選秀重排                   | 47      | love1500274  |
| \[Live\] 小牛 @ 巫師                                | 44      | Rambo        |
| \[Live\] 黃蜂 @ 溜馬                                | 2       | Rambo        |
| \[新聞\] 他倆在湖人最高薪　球團卻叫他們「坐」這     | X4      | zzyyxx77     |
| \[Live\] 湖人 @ 火箭                                | 70      | Rambo        |
| \[Live\] 拓荒者 @ 馬刺                              | 爆      | Rambo        |
| \[新聞\] 柯神和湯神　誰是艾倫心中最強的射手？       | 99      | kirbya       |
| \[新聞\] 狀元熱門老爸又開砲　這回竟槓上詹皇和       | 71      | HANASUCIA    |
| Re: \[情報\] 巴克利說球爸打不贏他                   | 32      | Rubio5566    |
| \[新聞\] 庫班要賣10%股份給歐巴馬　但竟開價… 4       | A       | ssisi        |
| \[Live\] 國王 @ 太陽                                | 5       | Rambo        |
| \[Live\] 公鹿 @ 快艇                                | 34      | Rambo        |
| \[新聞\] 沃爾爆料 卡森斯被交易前想去巫師            | 43      | pneumo       |
| \[討論\] 想請問一下 Ball 的爸爸                     | 23      | MLbaseball   |
| \[新聞\] NBA「駭客」馬里安 下週末起訪台6天          | 60      | jailkobe5566 |
| \[情報\] 籃網今夏將追求George Hill？                | 爆      | Yui5         |
| \[新聞\] 錢德勒生涯下一步 幫助太陽新秀成長          | 45      | thnlkj0665   |
| \[討論\] 臂展~長度優勢                              | 14      | checktime    |
| \[BOX \] Mavericks 112:107 Wizards 數據             | 41      | Rambo        |
| \[BOX \] Jazz 97:83 Pistons 數據                    | 19      | Rambo        |
| \[BOX \] Timberwolves 104:117 Celtics 數據          | 57      | Rambo        |
| \[BOX \] Hornets 77:98 Pacers 數據                  | 44      | Rambo        |
| \[BOX \] Pelicans 112:120 Heat 數據                 | 44      | Rambo        |
| \[BOX \] Blazers 110:106 Spurs 數據                 | 60      | Rambo        |
| \[BOX \] Grizzlies 98:91 Bulls 數據                 | 48      | Rambo        |
| \[BOX \] Lakers 100:139 Rockets 數據                | 97      | Rambo        |
| \[BOX \] Kings 107:101 Suns 數據                    | 40      | Rambo        |
| \[閒聊\] Dragic提到被呼喊MVP                        | 74      | dragon803    |
| \[BOX \] Bucks 97:96 Clippers 數據                  | 81      | Rambo        |
| \[花邊\] D'Antoni：現在對Kupchak和Buss是段艱難      | 22      | Yui5         |
| \[外絮\] 香波：交易到騎士把我從地獄拉出來           | 爆      | kyle5241     |
| \[新聞\] 大讚林書豪 羅培茲:他讓我們提升兩個層次     | 84      | jhemezuo     |
| \[花邊\] 低迷主因？柯瑞：球迷派兒子約我女兒         | 31      | adam7148     |
| \[新聞\] 公牛遭灰熊毒手 Wade:時間所剩不多           | 14      | Angel0724    |
| \[新聞\] 影／別擋詹皇路！ 隊友厄文快攻無辜遭撂      | 11      | adam7148     |
| \[情報\] Eric Bledsoe 本季提前關機                  | 70      | thnlkj0665   |
| \[討論\] 歷史上有比籃網還悲劇的球隊嗎？             | 24      | f22313467    |
| \[討論\] 沒拿過冠軍的球星你覺得最可惜的是？         | 爆      | dlOvOlb      |
| Re: \[討論\] 沒拿過冠軍的球星你覺得最可惜的是？     | 爆      | zakijudelo   |
| \[情報\] 今年雙十前段班數字已經超越上季             | 10      | carotyao     |
| \[花邊\] JR:我復出以後James就一直大三元             | 12      | bigDwinsch   |
| Re: \[討論\] 沒拿過冠軍的球星你覺得最可惜的是？     | 5       | wn7158       |
| \[花邊\] Whiteside談Dragic眼傷：告訴他如果看見      | 76      | vm04vm04     |
| \[情報\] Earl Watson教練談關機Bledsoe               | 12      | dragon803    |
| Re: \[外絮\] 香波：交易到騎士把我從地獄拉出來       | 32      | carotyao     |
| \[花邊\] Dirk收到球迷特別禮物:印有照片的馬鈴薯      | 24      | Kay731       |
| \[新聞\] 衛少大三元太瘋狂 德羅展：我是他球迷        | 40      | manuginobii  |
| \[討論\] 雷納德輸了這場仗 也輸掉了今年MVP           | X1      | jojoii82     |
| \[新聞\] 想讓隊友搶籃板 吉諾比利「丟」罰球卻        | 15      | zzzz8931     |
| \[情報\] NBA Standings(2017.03.16)                  | 27      | kadasaki     |
| Re: \[討論\] 雷納德輸了這場仗 也輸掉了今年MVP       | 15      | schume0417   |
| \[新聞\] NBA／Batum偏頭痛缺賽 黃蜂慘遭溜馬踐踏      | 13      | iam168888888 |
| Re: \[討論\] 姆斯守的住喬丹嗎？                     | 14      | lavida       |
| \[討論\] 雷納德慘遭Pop飆罵                          |         | jack0506000  |
| \[討論\] 哪個球員助攻最強？                         | X1      | star1739456  |
| \[討論\] 雷霆需要哪些補強?                          | X2      | sunnycello   |
| Re: \[討論\] 雷納德慘遭Pop飆罵                      | 22      | ymgs1507     |
| \[新聞\] NBA》傳籃網想要爵士控衛 林書豪上場空       | 6       | lovea        |
| Re: \[討論\] 雷霆需要哪些補強?                      | 8       | f22313467    |
| Re: \[討論\] 雷霆需要哪些補強?                      | 5       | s106667      |
| \[外絮\] Kawhi Leonard的邁向球星之路                | 35      | jimmy5680    |
| \[情報\] Griffin和Jordan輪休                        | 19      | s870098123   |
| \[討論\] 故意傷人的罰則                             | 1       | hhll5566     |
| Re: \[新聞\] NBA》傳籃網想要爵士控衛 林書豪上場空   | 31      | nuturewind   |
| \[情報\] Wade本季報銷                               | 爆      | s90523       |
| \[討論\] Skal Labissiere                            |         | slamblock15  |
| \[心得\] 1000英哩的長征--NBA主場朝聖之旅            | 爆      | Hangibb      |
| \[花邊\] 太陽總經理：L.Ball的父親不會影響我們做決定 | 12      | bigDwinsch   |
| \[花邊\] Westbrook對Curry的MVP看法表示：他誰？      | 爆      | hpan0806     |
| \[討論\] Kevin Love將在今天對爵士的比賽復出         | 15      | s90523       |
| \[Live\] 爵士 @ 騎士                                | 67      | Rambo        |
| \[Live\] 雷霆 @ 暴龍                                | 29      | Rambo        |
| \[情報\] 火鍋排行~Gobert榮登店長寶座                | 19      | checktime    |
| \[Live\] 籃網 @ 尼克                                | 爆      | Rambo        |
| \[Live\] 灰熊 @ 老鷹                                | 5       | Rambo        |
| \[閒聊\] 灰狼隊的Nemanja Bjelica整季報銷            | 31      | dragon803    |
| \[新聞\] 隨隊記者爆料　勇士球員零互動               | 34      | murray       |
| \[Live\] 快艇 @ 金塊                                | 7       | Rambo        |
| \[新聞\] 公牛GG了　韋德肘傷整季報銷                 | X1      | tsukiji      |
| \[花邊\] 榜眼變農夫 米立西奇毀了自己                | 74      | bigbearee    |
| \[外絮\] Jordan Crawford 與鵜鶘隊簽下兩年合約       | 23      | thnlkj0665   |
| \[新聞\] 魏德談骨折「有季後賽且醫生准許就復出」     | 21      | TeacherLin   |
| \[專欄\] 總冠軍、MVP、老八與水蛙季末大預測LYS       | X9      | wayne64001   |
| \[Live\] 魔術 @ 勇士                                | 99      | Rambo        |
| \[新聞\] 林書豪15分8助攻 籃網一周內2勝尼克          | 爆      | jimmy5680    |
| (本文已被刪除) \[qk56\]                             | X1      | -            |
| \[討論\] 甜瓜放棄logo shot                          | 35      | yangsungo    |
| \[BOX \] Nets 121:110 Knicks                        | 爆      | checktime    |
| \[BOX \] Thunder 123:102 Raptors                    | 84      | checktime    |
| Re: \[討論\] 甜瓜放棄logo shot                      | 90      | carotyao     |
| Re: \[討論\] 閃電俠Wade的負面新聞是不是有點多?      | X1      | overkill0802 |
| \[討論\] curry是不是曇花一現的最佳例子？            | 7       | n123033401   |
| Re: \[討論\] Rubio今年FG%能跨越4成線嗎?             | 43      | carotyao     |
| Re: \[花邊\] Westbrook對Curry的MVP看法表示：他誰？  | 29      | KirkSynder   |
| \[情報\] Melo：這是我們最糟糕的賽季之一             | 91      | vm04vm04     |
| \[討論\] NBA近幾年最強的選手                        | 99      | One102bird   |
| Re: \[討論\] 甜瓜放棄logo shot                      | 31      | aifighter    |

解釋爬蟲結果
------------

``` r
nrow(NBA_dataframe)
```

    ## [1] 100

總共有幾列 就是有幾篇文章標題 總共100篇

``` r
sort(table(NBA_dataframe[3]),decreasing =TRUE)
```

    ## 
    ##        Rambo    checktime     carotyao   thnlkj0665    dragon803 
    ##           22            4            4            3            3 
    ##     kyle5241         Yui5     adam7148    f22313467   bigDwinsch 
    ##            2            2            2            2            2 
    ##     vm04vm04    jimmy5680       s90523       Assisi    HANASUCIA 
    ##            2            2            2            1            1 
    ##   hitman0527 jailkobe5566       kirbya  love1500274   MLbaseball 
    ##            1            1            1            1            1 
    ##       pneumo    Rubio5566    Vedan1213     zzyyxx77    Angel0724 
    ##            1            1            1            1            1 
    ##     jhemezuo      dlOvOlb iam168888888  jack0506000     jojoii82 
    ##            1            1            1            1            1 
    ##     kadasaki       Kay731       lavida  manuginobii   schume0417 
    ##            1            1            1            1            1 
    ##  star1739456   sunnycello       wn7158     ymgs1507   zakijudelo 
    ##            1            1            1            1            1 
    ##     zzzz8931      Hangibb     hhll5566     hpan0806        lovea 
    ##            1            1            1            1            1 
    ##       murray   nuturewind      s106667   s870098123  slamblock15 
    ##            1            1            1            1            1 
    ##            -    aifighter    bigbearee   KirkSynder   n123033401 
    ##            1            1            1            1            1 
    ##   One102bird overkill0802   TeacherLin      tsukiji   wayne64001 
    ##            1            1            1            1            1 
    ##    yangsungo 
    ##            1

Rambo最多總共22篇 Rambo在NBA板上主要是處理Live以及Box情報 所以文章數最多
