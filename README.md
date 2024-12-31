# 开源项目多维度健康评估与可视化

## 一、项目背景与简介

**项目背景**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在当今数字化时代，开源软件已经成为技术创新和软件开发的核心驱动力之一。从操作系统到编程语言，再到各种应用程序和服务，开源项目几乎触及了信息技术的每一个角落。随着越来越多的企业和个人依赖于开源解决方案，确保这些项目的健康性和可持续性变得至关重要。然而，开源项目的健康发展面临着一系列挑战。许多项目缺乏透明度，使得外部用户难以评估其内部运作、代码质量和社区支持等关键因素。
**项目简介**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;《开源项目多维度健康评估与可视化》，基于数据相关技术，对开源项目进行数据分析，进而以可视化形式展示，旨在为开源项目的健康状况提供一个全面、客观且易于理解的评估框架。通过引入合理的多维度健康评估体系，结合可视化技术，帮助开源社区、开发者以及企业用户更好地理解和衡量开源项目的实际状态。
## 二、设计思路
### 1.数据获取与预处理
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;解析 OpenDigger 下载的JSON数据文件，转化为CSV文件，同时，根据文件路径名称为CSV文件命名，确保每个CSV文件准确反映其对应的项目，具体指标和指标数据。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对各个指标数据进行预处理操作：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;① 对于Opendigger中缺失的日期数据，统一日期标准，对同一开源项目中的所有指标文件进行补齐。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;② 考虑数据的性质与特性，对上述日期补齐的数据做不同的缺失值和异常值处理。
### 2.多源数据融合
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对预处理后的数据进行融合，综合考虑OpenDigger中指标数据的性质、应用和计算资源等因素，并综合数据之间的相关性，依据**项目活跃度，社区参与度，代码质量，用户反馈**这四个维度层面考量。此外，对于不同数据格式，复杂的数据等情况，需要对其进行转换，从而对多源数据的精准高效融合。另外，还应用一种全新的评估来统一上述四个维度层次，使得原本分散的指标数据能够在同一维度上进行比较和综合分析，进一步提升了数据的可读性和可解释性。
### 3.Dataease 可视化大屏
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DataEase 具备丰富多样的功能与特性，能够高效地处理各类复杂数据，并将其转化为直观形象的图表形式，便于用户理解和分析。将预处理及融合的数据源导入到该平台中。依据数据的类型，分析的目的，选择合适的图表类型，设置图表的各项参数。确保每一张图表都能够精准无误地呈现数据的核心要点，最大程度地提升数据的可视化效果。

## 三、健康评估模型

### 1.项目活跃度评分
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;项目活跃度是衡量一个开源项目在其生命周期内是否保持积极发展和社区参与程度的重要指标。它反映了项目的活力和健康状况，对于评估项目的长期稳定性和可靠性至关重要。包含以下四个维度：
#### （1）提交频率
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;提交频率是衡量开源项目中代码更新活跃度的重要指标。它反映了开发者团队在特定时间段内对项目代码库的更改次数，是评估项目健康状况的关键因素之一。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计算方法：基于"active_dates_and_times"中的时间戳统计开源项目特定时间段内的提交次数，并映射到相应分数区间。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;排除大部分用户不活跃的时期，并基于提交次数计算提交频率分数，定义每月600为提交满分，简单直观的衡量了提交频率的分数，体现项目的提交活跃度。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**公式：Time=sum(s_active/600)**
#### （2）贡献者分析
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计算方法：综合分析 ‘新贡献者new_contributors’“不活跃的贡献者 inactive_contributor ’贡献者contributors` ‘bus_factor’，’参与者 participants ’ 五项指标数据
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.Con占比评分（1/2）**：**s_con=contributor/bus_factor**反映的是贡献者相对于 Bus Factor 的比例，评估项目对少数核心贡献者的依赖程度。.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.Bus_factor占比评分（1/6）**：**s_bus=bus_factor/participants**，表示 Bus Factor 占总参与人数的比例，有助于了解项目对抗风险的能力。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3.New占比评分（1/6）**：**s_new=new_contributors/participants**显示新贡献者占总人数的比例，体现了项目的吸引力和包容性。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**4.Inact占比评分（1/6）**：**s_inact=inactive_contributor/participants**：衡量不活跃贡献者占总人数的比例，揭示了潜在的风险或社区活力的问题
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**公式：Activity={s_con/2+[(1-s_bus)+(1-s_inact)+s_new]/6}*10**
#### （3）合并请求处理效率
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;合并请求处理效率是衡量开源项目中，团队对合并请求的响应速度和处理能力的重要指标。它反映了项目在代码审查、反馈提供以及最终合并或关闭请求方面的效率，直接影响项目的开发进度和技术质量。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计算方法：综合分析’change_request_resolution_duration’ ‘change_request_response_time’   ‘change_request’ ‘change_requests_accepted’，计算平均响应时间和接受率。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1.接受率（1/3）**：request=accepted/request当接受率高的时候，可以说明相对的合并内容较多，该项目的合并活跃度比较高。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2.平均反应时间（1/3）**：response_time，反应时间快可以反映项目的合并速度会相对较高，反映项目能获得的重视程度。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3.解决持续时间（1/3）**：duration，解决持续时间可以反映项目解决的速度，同样能反映项目获得的重视程度。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**公式：Request=(request+response_time+duration)/3**
#### （4）项目死亡评分和项目规模评分
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;用于评估软件项目健康状况
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**项目死亡评分**：项目死亡评分用来衡量一个项目可能停止活跃或不再维护的风险，即项目“死亡”的可能性。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计算方法：统计项目中各时期参与人数少于最大参与人数的0.1倍的时期数 death。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**项目规模评分**：项目规模评分用来衡量项目的大小或复杂度，通常反映了项目所涉及的工作量和技术难度。大项目往往具有更多的功能模块、更复杂的架构以及更大的用户基础。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计算方法：统计项目的时期数，并按照10^1 10^2 10^3 10^4 10^5 分别评估size

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**项目活跃度维度分数为： act_final=(time+activity+request-0.4*death)/3
**
### 2.社区参与度评分
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;社区参与度是衡量一个开源项目中用户和贡献者活跃程度的重要指标。它反映了社区成员之间的互动频率、合作深度以及对项目的投入水平。高社区参与度不仅有助于项目的快速发展和技术进步，还能增强项目的稳定性和长期可持续性。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计算方法：综合分析’问题评论issue_comments.csv’、’问题解决持续时间issue_resolution_duration.csv’、问题响应’issue_response_time.csv’、新问题’issues_new.csv’ 和 ‘已关闭的问题issues_closed.csv’ 五项指标数据

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. Closed占比评分（1/6）**： s_closed=closed/comments，衡量已关闭的问题在所有问题中的比例。较高的Closed占比表明项目团队有效地解决了用户提出的问题。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2. New占比评分（1/3）**：s_new=new/comments，反映了新提出的Issue在所有Issue中的比例。它可以帮助了解项目的活跃度以及是否正在引入新的问题或功能请求。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3. 反应评分（1/4）**：s_response衡量的是项目维护者或其他贡献者对Issue的响应速度。快速的反馈可以提高社区的信心并促进更快的问题解决。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**4. 持续时间评分（1/4）**：s_duration持续时间评分考量的是每个Issue从创建到最终关闭所需的平均时间。较短的持续时间通常意味着更高的效率。
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **社区参与度维度分数issue=s_closed*10/6+s_new*10/3+s_response/4+s_duration/4**
###    3.代码质量评分
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   代码质量评分是评估开源项目中代码库健康状况的重要指标。它反映了代码的可读性、可维护性、安全性。一个高质量的代码库不仅有助于项目的长期稳定发展，还能提高开发效率和减少潜在的技术债务。
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  计算方法：综合分析 '增加代码行数code_change_lines_add' ’删除代码行数code_change_lines_remove‘'改变代码行数code_change_lines_sum ’‘变更请求审查change_requests_reviews‘ ’变更请求change_requests’ ’接受的变更请求                                                                                                                             change_requests_accepted‘
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. 新增与删除代码行数的比例（1/2）**：s1=(add-remove)/sum评分反映了代码库中新增和删除代码行数之间的平衡，用以衡量代码库的维护状况和发展趋势，最为重要。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2. 变更请求接受率（1/4）**：s2=accepted/request 变更请求接受率是指被接受的变更请求数量占总提交变更请求数量的比例，用以评估代码审查的有效性和团队协作的质量。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3. 审查人员分布（1/4）**：s3=review/request 审查人员分布反映了参与代码审查的不同贡献者的多样性，用以评估审查过程的公平性和广泛性。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**   代码质量评分：quality=s1/2+s2/4+s3/4**
###  4.用户反馈评分
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  用户反馈评分是衡量开源项目用户体验和社区满意度的重要指标。它综合了多个数据源，旨在评估用户对项目的使用感受、遇到的问题以及他们对未来发展的期望。项目团队可以更好地理解用户需求，优化功能，并增强社区的参与度和支持力度。
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 计算方法：’参与者participants’ ‘项目关注度attention’ ‘问题数issues’ ‘问题评论数issue_comments’ ‘星值stars’ ‘fork值 technical_fork’  ’已关闭问题issues_closed ’
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;** 1.Closed评分（1/9）**： s_closed=closed/comment关闭问题的数量和速度反映了团队对用户问题的响应能力和解决效率。
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **2.New评分（2/9）**：s_new=new/comment新增的问题和评论提供了宝贵的用户反馈，特别是建设性的意见有助于改进产品和服务
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  ** 3.Attention评分（1/3）**：s_attention=attention/participants反映了相对于参与者数量的关注度水平，有助于评估项目的外部吸引力和影响力，还可以揭示潜在的问题和改进机会。
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**  4.Stars评分（1/6）**：s_star=star/attention直接反映了项目的Star值，用以衡量项目的受欢迎程度。
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  **5.fork评分（1/6）**：s_fork=fork/attention反映了项目的Fork值（即复制数）与关注度的关系，用以评估项目被实际应用和改进的程度。可以衡量复制对关注的影响倍数，从而得出复制的实用性和影响力
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ** 用户反馈评分：Feedback=s_closed/9+s_new*2/9+s_attention/3+s_star/6+s_fork/6**

 &nbsp;&nbsp;&nbsp;&nbsp;**最终评分 health=act_final*0.3+quality*0.3+issue*0.2+feedback*0.2**

## 四、技术难点
### 1.数据的预处理
#### （1）批量处理csv数据
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**① 处理 json文件**
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;将 top_300_metrics 文件中所有的 json 文件都转换为 csv，手动操作效率总是极其低下的，此外，为了区别不同开源项目中相同名称的指标数据，每个 csv 文件的名称都必须包含其项目名称。
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;具体细节：
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;函数接收根目录路径('./top_300_metrics')和输出目录路径('./outputdata')作为参数。使用os.walk()遍历根目录及其所有子目录，对于每个子目录，获取其名称以及父目录的名称。对于每个子目录中的JSON文件，根据父目录名、当前子目录名及文件的基本名称来构造新的CSV文件名。之后调用①（parse_opendigger_json）解析JSON文件内容，再通过②（write_to_csv）将解析数据写入新命名的CSV文件中。
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**② 处理contributor 数据**
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;注意到在opendigger 文件数据中存在“contributor_email_suffixes”指标数据，标识贡献者（contributors）电子邮件地址的域名后缀，而缺少了具体的贡献者数量”contributor”数据，因此，需要将“contributor_email_suffixes”中的贡献者数据统计并转化成 “contributor” 数据，构建一个新的贡献者指标数据。
#### （2）补齐缺失数据
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**① 补齐缺失日期**
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;实现了批量处理CSV文件，确保每一组CSV文件中包含特定的日期列，并且这些文件中的日期条目与基准时间（active_dates_and_times）列表相匹配。对于每个需要处理的CSV文件，如果缺少指定的日期列（'指标名称'），则会添加该列并用NaN填充。将文件中的日期格式统一，并通过合并操作来检查和补全任何缺失的日期条目，确保所有文件都包含了完整的、与基准时间列表一致的日期序列。
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**② 补齐缺失元素**
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;出现以下两种情况
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**a.该时间段确实没有具体数据产生而缺失时间数据，对应的指标数据值补0：**
  &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;使用 fillna(0) 将相关数据中数据值为 NaN 的补齐为 0
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**b.时间段由于出现特殊情况导致没有数据收集，产生数据缺失：**
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**使用平均数，众数等缺点**：当数据值起伏波动交大时，后期的大数据的平均数，众数会影响到前期小数据的情况，这里与实际情况不相符程度较大。
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**使用线性回归模型缺点**：当数据变化波动大等情况，易于导致其他填补的数据出现负值（实际情况不可能出现负值），而造成数据的填补不当
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;这里采用模型的方法：**K 近邻（KNN）填补**
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;KNN 算法可以根据样本之间的相似性，使用最相似的 k 个邻居的平均值来填补缺失值。对于时间序列数据，根据时间相近的数据点作为邻居，这样的数据更切合实际情况。
#### （3）复杂指标数据的处理
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;问题，变更请求响应时间与解决持续时间这类复杂数据，则直接将其 avg 中的参数进行平均值求法，最终可进行评分处理。
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;此外，还需正确解析CSV文件中上述指标数据元素值，提取数值为 (datetime, float) 对，按日期排序后分离出两个列表分别存储时间点和数值。
#### （4）数据存放文件
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;为了确保计算评分结果能够有序且准确地保存在文件中，首先需要对输出文件进行初始化设置，对该文件建立一个清晰的结构，预先设定好所有开源项目的名称，以保证后续的计算数据记录对应。为每个维度都建立一个对应的csv文件，同时再建立一个保存所有维度，指标数据的文件供信息可视化。
### 2.融合数据算法
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;对于预处理的数据，融合策略选择复杂，要综合考虑数据性质、应用需求和计算资源等因素。并且融合算法计算复杂，对计算资源要求高，对于不同数据格式等情况，需要复杂的数据转换工具，处理大规模数据时计算负担重。
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;依据健康评估模型进行融合数据。综合洞察评分。多源数据的整合为用户提供了一个全面的综合视角，能够更深入地理解项目的各个方面。
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**融合过程缺失文件处理**：
 &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;当某些文件不存在时所采取的应对措施。无法搜索相应文件路径时，采用填充的方法，np.zeros将数据补充完整进行运算，继续处理其他可用数据，对于其他一些复杂数据的缺失，则采用特定的判断，对该维度的评分进行默认设置，防止因数据缺失而导致代码编译错误，
### 3.DataEase可视化大屏
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**准备数据源：**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;将计算的所有数据保存在一个csv文件中，并且计算出最终评分，然后通过DataEase的数据源管理界面连接此CSV文件创建数据集（all），指定数据类型和主键以确保正确解析，以便构建可视化大屏。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**可视化大屏设计：**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;选择合适的可视化大屏模板，规划整体布局，添加含有上述数据集的明细表，供全局浏览所有开源项目相关维度数据。此外，还添加查询组件与之相联系，能够按照项目名称查询特定的维度与健康度数据，在明细表的指标一栏，可以根据不同的维度对所有开源项目进行升序或降序排序，以便更好理解与分析不同维度的指标。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;同时，对于一些更直观的opendigger 指标数据，例如，star值，贡献者数量，fork值还有统计月份，则是构建指标卡直接展现，并且同样可以随着查询组件对所有开源项目的上述数据进行查询，更加直观地感受数据。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;另外还通过条形图直接展示了 Top10 项目的健康度和项目贡献者数量，以及统计的项目规模数，最后将计算的四个维度（项目活跃度，社区参与度，代码质量，用户反馈）和项目健康度的评分中位值展示，表现维度的中等水平，明确每个开源项目的维度是否位于中等水平之上。
## 五、可视化展示与功能
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**整体呈现：**

![](https://www.helloimg.com/i/2024/12/31/677401eabaf0d.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**相关功能：**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**查询功能的实现**，查询组件位于左上角，能够根据项目名称查询每个项目的指标评分以及最终计算的健康度评分，此外，位于评分表上部分的四个指标数同样与查询组件相联系，当查询时，该指标数会随之改变为对应项目的指标值总数。

![](https://www.helloimg.com/i/2024/12/31/677401ea45d8f.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**表头排序：**可以根据每个表头指标对所有的开源项目进行排序（升序或降序），迅速洞察开源项目的不同表头指标，从而更快捷地获取特定信息。

![](https://www.helloimg.com/i/2024/12/31/677401ea1bfe5.png)

### 动图展示：
**整体呈现：**

![](https://www.helloimg.com/i/2024/12/31/677403be76058.gif)

**查询功能：**

![](https://www.helloimg.com/i/2024/12/31/677403be8ef7a.gif)

**表头排序：**

![](https://www.helloimg.com/i/2024/12/31/677403bd645fe.gif)
