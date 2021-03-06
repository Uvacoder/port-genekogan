---
layout: work
title: Wiki t-SNE
description: A t-SNE of Wikipedia articles of political ideologies
year: 2016
thumbnail: /images/home/thumb_wiki-tSNE.jpg
redirect_from: /works/wiki-tSNE.html
---

{::nomarkdown}

<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.5.6/p5.min.js" type="text/javascript"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.5.6/addons/p5.dom.min.js" type="text/javascript"></script>
<script>

/*
wiki-tSNE:: Wikipedia articles of political ideologies / philosophical concepts.
Converted to tf-idf matrix, then clustered by t-SNE.
*/

// first time you load the data, the exact positions have to be adjusted so set adjust to true
// after that, move the downloaded adjusted json file to the sketch folder and set adjust to be false
var filename = 'data_political';
var adjust = false;

// parameters
var zoom = {x:1.5, y:3.0};
var txtSize = 20;
var margin = {box:8, l:80, t:50, r:350, b:70};
var tx = 120;
var ty = 85;
var infoMargin = 36;  


////////////////////////////////////////
var canvas;
var data;
var boxes;
var info;
var tl, br;
var highlighted;
var infoHighlighted;
var ox, oy;

function preload() {
  data = {
    "x": [822.4844131694616,502.21583149251813,195.22994525875572,1640.9202774843618,500.30283734898256,577.2144614739507,1724.9233349841898,1700.6627082188272,1640.3742069382324,2193.5938979872853,1512.1976354705312,803.3949625215661,1383.7816332977047,942.7094054219924,1777.6284339862696,891.4588078718392,305.9136287519696,2094.916242959656,670.6085676457217,1158.8575046686792,1059.3967403404165,1251.7031562615675,1151.5731570271246,1149.585766740069,1922.1266931810962,1306.6767171145173,618.3321292833094,848.9834479688236,496.02767062292247,1250.8922237961085,1036.3888976204312,717.3640139421667,1175.9785315428726,1302.2545639562898,968.9095675844194,929.0176236065233,869.6056731002116,316.83923903544985,1465.3931934206796,550.2479682594177,169.51834403908836,1354.823689522166,1508.3541390196172,13.707309834888871,764.0658543871383,1428.5955176070722,1484.3234087385474,1088.3929474604886,970.992616238142,2099.353307569229,825.5364909466231,2128.614543840349,147.59602809837082,230.53525414064993,1447.8937203137125,1520.9990062327965,1603.7427848961497,242.55423249215923,977.3712734833645,1697.4267192245745,712.8416958788252,552.6698097173858,434.22372061444077,1393.6718839474622,762.5599774796974,805.1285168314764,1090.3667924861036,1595.2827685024993,1014.6063402890366,1637.5664568932757,1465.0703888199873,626.5655384131528,1575.5477685013825,437.03473748858175,1512.7620912283742,1451.2810320133601,1166.662870636102,1632.5679769645885,1177.3661034161607,1536.7994132539116,1692.1503457306767,934.5081073263293,1268.210044755968,1585.9303168421898,1730.4414608707957,1066.5818200317935,259.15285538908756,1349.3501396655058,1901.2608284367661,2091.0158555370695,1167.6382642978995,1499.7350490434408,467.35681787611657,126.20918350787481,1364.3499491794823,1920.301348754335,1173.5851060661214,2069.3101819464846,1639.2623062381065,796.0070996364568,312.7550244316049,520.9301088017776,318.40653046499904,397.3602197572315,959.9440095073812,1073.4406226107178,1073.1833609799573,1577.987810077716,1210.4377790927404,1124.3235527231836,746.6020108706501,1073.7356109585544,1021.0237384067368,1321.8107739351553,958.2194178018657,707.0061293049943,771.8893721971656,1162.23929082737,1367.8238722650906,1105.6804583274813,2153.965625441689,842.0371917979301,1910.532465903586,1148.3104908513535,1595.3612593709754,2069.067534522519,1733.2882632138233,454.9585585961603,650.830662098484,738.4546783868291,1286.3075335106773,370.68581890714285,1905.2561201286587,1106.7668451799389,1683.9314052065724,1766.7210430264338,273.91119883962796,1735.7830364666202,1477.183724338733,2013.704662827486,1229.4493825006482,1703.4689746143547,839.4883560220381,1342.7713887338641,293.41639897600993,979.3634969806589,1597.5485260535074,1365.5294448759469,1154.0068862652975,21.805826286093748,1820.7352601538441,1342.6779512056994,1185.2999261090529,1251.9307761587947,1463.3386642470653,1634.8414101036947,814.06809209584,878.5003702927827,1102.070161409732,967.5574032390219,825.4342817879211,959.9523150857424,905.9013344778687,1802.5705360137474,969.0496030078801,606.7574608170248,999.5013680166625,1282.5329068691105,1859.7768521608627,1052.8708918095365,1489.6076386773625,2167.8507836440244,2263.458032827466,619.2708746265764,1210.9827914902871,793.9948240340644,1061.2234020832316,493.6366574273674,209.6627968932312,640.3331787866989,455.1346409609328,1046.1460129524246,1017.0344214304356,1083.7259561791175,1351.0940421565226,864.575194544826,1949.6113779240502,641.7847673469666,1944.8979568667748,291.4341905371509,1568.5379662707169,2216.555165832477,696.1858484312273,968.8030511232167,1060.456831543583,723.263561309051,1340.8073411806251,1530.3387371195406,2054.127674430584,1062.4779693972575,717.5939002430398,813.076620817771,858.4198389762786,1804.882080577845,1887.6923939117885,371.5403326936385,1456.5437532103344,1491.3913034487089,1061.8872124299921,938.2597520465691,752.7253708188264,690.2629968326823,579.3157120106225,967.4119153865755,284.8362037913258,1304.1373044996199,516.7867828120318,1103.9687031802578,629.251915493158,1353.316423737601,1024.3609535661851,1658.3132518322307,426.73387176343,1258.283791401683,314.39913798559587,395.7403562368569,168.4122501558453,180.82926844078227,979.1852991680789,1967.3858270270143,1430.421905451721,857.3622883566577,589.7446604544622,895.1823271127237,1449.4035041178267,625.271660618949,1844.9216293171803,1010.7323097241244,680.8040106567726,1824.0750872755832,1279.357660218628,851.3795670061688,1158.9534221783722,1877.7681893590245,1262.7698002837453,1592.5429441745505,1790.5768205339375,230.71309447886435,1598.4980068641446,544.1404349967136,222.31345346642354,2130.3886880286923,691.9037645808583,1535.57224502135,1631.0776645149213,821.297752629746,1105.04078007119,1251.2309230217418,1322.0045268664662,1123.7633631885299,1701.4535764778886,639.7656590307844,1256.6415561062063,1293.4155994796808,1209.6277781142257,1183.8036711126554,727.2755353705708,1476.5513415135522,101.16394508924385,986.525493439351,415.5014167643971,568.001619928387,1733.4658650240904,474.79986194123387,1542.136049123256,1624.0157860341087,1476.385131993704,1865.9048906161054,1005.9037684309675,847.8387915048146,1642.2991353124041,1204.136193365371,1479.394655765273,462.4521759260049,874.1794451603288,1354.02463607512,939.1788104077884,1143.8990009669142,586.3128724489841,861.8199984258899,1945.4632251076864,2067.0780512535407,1814.776388979939,934.4094664191367,796.8338553198398,342.29988201977693,2137.8097789408325,1292.0494333543538,729.144140477585,1057.3564191840978,1293.5448584387584,1788.236671347401,664.3384517188244,916.8780141362821,1947.987506335538,1464.7095492205947,1780.2951461094017,1197.994954903027,1955.5025489723598,500.14929239738797,1002.4569005274775,1469.307181079253,1947.1266063478001,902.3167921471796,507.52265504178496,537.749850317073,640.9884594686376,1259.8338271978173,1487.1499169270116,1874.772171612326,1107.6310342501165,1352.6820580214849,1587.6608970480825,1213.0895062771576,1719.5423636943449,820.5770354798659,1094.2447955796185,352.97910400213186,720.7230559152009,840.7213255927124,1166.8820776321718,1649.4081372975506,0,1235.0684358438186,1193.404524033941,390.1071679219531,786.3901080472672,1617.4689264721383,758.469467807347,1501.388606213021,201.99728350931454,1923.9010888896191,935.191290251136,1331.2419920254263,1238.4863952070707,968.1886912876035,1019.8143388175984,1297.573812860604,1501.243436485911,1331.625843929575,425.1483904533756,371.61270824273606,1805.56996020652,601.9696182339396,1872.0714926710355,1773.1642782390218,368.469932939518,1323.5635412071465,1216.0239700550671,1293.7077649661542,501.62346141745473,827.7292452107288,1203.5864678165349,1911.6350005145805,1137.5516082087427,830.5025414431418,214.89870779597402,1403.1549048955205,1980.2136883833302,549.806145467864,13.098559032601047,654.7792866621031,1179.265795789718,539.7229396278852,384.2550101296728,728.7729491936462,461.5599565632532,1003.3007963119024,1426.200851512138,327.33188833223653,1717.3077974958587,547.9105743670113,478.05775516083634,1586.6937712127674,786.6471067027388,860.8996072291206],
    "y": [1056.4410123880496,1200.3373636016754,1070.4915468903534,1323.3283311661016,1163.9292595019554,1928.6970338907504,467.57066601256327,1273.3417617578164,1863.111832204298,918.6049330161953,1769.3285431897136,2097.4246676137554,1323.1131653263233,1443.8278074963494,671.1766409866613,1606.3794369971806,711.8844324072384,1144.1546676014336,1892.1806733793737,1512.675003305616,868.4576489562478,647.9112312032746,2039.9308509108034,1678.6090645712925,1468.4914699134365,1491.687663674698,971.3616920847694,1818.7318353626454,1351.7218527883015,1592.0906004077037,717.8479125945968,1574.8623605599817,218.26392501378962,1213.806664693045,754.0115809961012,1979.8617945649364,2178.9673510404527,1034.2740462660468,1580.8010936200508,1737.6673435094947,1789.9782953946883,1380.472035002001,334.3278865984936,1086.6540364620944,946.5219628963595,1193.8331185700818,1396.5082198320006,2219.8957652316544,1370.9301701554396,1068.0464947125581,1298.6553024423063,728.2082858216532,1322.102156289495,1147.5860571510332,382.4055633589987,1680.976084538116,1644.7075592514736,855.4008139513691,2267.9873779517993,1733.1933131390988,824.0800047188421,854.8043306984349,1091.4497595908315,902.1899420514976,1484.606975227166,667.4348442423261,1101.906151116506,2012.8942998199166,905.0468161340782,228.48108672251846,1967.2507451281379,1699.9723735633302,191.94866367704714,1127.523439941114,1642.356057982262,1029.5626493067864,1635.6985270705266,1819.8855333234346,508.92988671415753,1102.1768376680325,634.9819528712359,1191.4578725688707,982.9506331849528,741.1947197992363,1548.6729017581483,1055.6400689398786,1751.5847121886602,945.9486909528397,1624.9842422478255,1106.8262874963846,866.965156873222,1065.8636784501437,775.9241406872163,1285.6299435609922,1250.431831841342,1260.2388657121558,2297.892424537974,1217.6515197881083,1065.9874038009375,87.13517857745573,819.0566485389932,1404.3136226301517,782.3601687311289,1280.495965065361,1019.6742434913367,531.3232942543109,2022.700627729563,519.4394576023834,1065.3239154313046,1423.2357114311126,1448.0602083990557,790.6419270371306,0,572.1121426351423,1298.053952912217,1306.9310715222352,1093.6000887307687,1991.982317706452,792.7040609003226,1331.159073910212,687.147840034291,1407.6863365688803,2000.4101086938335,980.1313616709102,685.1925943012507,1180.3860101999442,1220.7953797757596,890.9063866604773,1269.9575715180747,1612.863040997657,1918.3359115494454,595.839464653203,1554.7560492456757,144.95051706548304,504.3832127910591,779.9359967642176,1470.0822130452132,1322.7098965363018,419.23911713620197,844.2681943321718,1752.1758302657577,1982.583634958246,1154.7413194703583,1861.0981092279362,1583.5424933357867,1521.1241927032493,1547.8003599408023,2061.365825832304,1897.9130663613091,1408.8158015846625,1791.0900431578307,2250.0349235851695,1250.4733699081933,1174.4449800823436,1527.0712007268473,956.488629803987,717.9288730766009,860.3536403110921,181.70029259466384,69.4410890581169,1334.878280812145,1483.7549194181013,395.21913317194105,1184.2390245203105,1334.797263900217,1616.255380696185,2168.776576406115,446.6733147243483,602.2451193214447,612.7246037916467,456.1306423054741,956.9441012128675,1250.9108720019653,1538.4085758216363,140.00707874597217,32.99359689293221,1825.091100498974,710.5624572872574,937.0861917561574,558.3103559255769,439.35501342791014,1184.591986361583,1147.2801115017257,375.24297962603777,1678.4147217778864,1485.9679600009838,1669.2495460075681,376.90942091558395,1347.6552986814763,1184.852068162099,644.4967885650778,1180.4039220121717,2023.8765957832425,1683.9385646758763,568.3954362372307,1720.5151283032624,1788.3942490322947,2228.067999770865,917.695513678727,1788.4032113245516,1408.2028264624764,631.0643596666889,787.8528099196288,1432.403865430762,489.023421005287,1417.4320345197702,993.2057020350594,1477.6502083159312,1019.3335830312622,145.15441009626994,468.6821308780245,1852.2541425803786,339.30873945412554,1913.8486794568096,1236.4913254556564,1454.0991651009072,1044.6872060255141,1287.1972287053538,504.87325767587123,2213.0485167759775,1407.705980814594,598.7382902092373,1629.5674985033284,409.56505775369806,1328.772942087786,1539.2252281807052,1029.0693751594843,1108.6137949164777,681.5863372621301,1107.2308310791886,756.6271325457441,1261.7119085483391,1856.0433760239966,1720.2893667977219,2087.596611628348,778.4729850290089,564.9378062800042,1751.632116174382,1652.6075641595078,954.8213692354361,1286.8343249630484,897.1861361591389,905.9609013201399,644.5621776502428,1028.5843255933382,873.6060002282538,1105.8991082382377,1923.4190832703641,836.1097966119987,1789.6953079022887,992.2510614524581,765.2206435195468,687.3472941957123,2049.2449576214153,1174.2684939923224,1224.6481527476867,468.8687973254837,610.514999087012,492.2823047221894,413.5718170356577,992.9772730813207,1056.0871287694472,828.3234058836802,1101.9698551829708,1138.14723483844,1861.2198083971814,1019.7052662316072,1717.6418298479214,1517.3614286575682,217.88918724635792,1826.1552453158952,1127.7477116057007,1029.6319844615457,1588.5538974125002,2190.982291837562,1396.759786916361,829.0139631115144,528.3536761905923,942.9784085196916,594.3230706490716,2140.8583858260013,2134.466404401559,1359.777169055166,1455.2378347651734,243.8117677679899,865.0847864178887,1861.62031963093,1367.7863211149659,672.2833748791743,431.83920180300277,1956.0097116058282,955.424905437601,1143.6048642901474,983.1641085736646,983.2037779923892,1922.6923000320764,1345.1308613880906,1548.991542634183,2060.717329523974,1642.3823671080368,1966.8707164867717,743.2766389012935,635.7568726754425,2132.4347406665543,1029.388705383061,1824.5750349525981,1919.882304198093,1714.7518831380203,711.2747354803661,1007.9044977623241,108.29078193124165,1899.3801112625256,880.8215715761498,1569.768822185751,609.621144675927,926.9822593295238,1200.079980062203,2097.5299730574206,1286.5649288026011,1224.1934594285797,1476.1975635543033,1138.2507348000274,1717.3604966312944,546.6229162469697,420.0387245966976,1371.2277915505001,1941.453703078358,334.8646537278373,1163.9488346908706,340.7009822440121,284.01796845081884,1458.811475645345,1130.0247195874829,338.900734099399,684.4819591424606,1719.7293589764975,1521.9959924381906,909.729345502503,1261.6546792692407,1229.8854423126218,900.9901733316049,797.5497110228207,1647.3954353714691,286.7008243053864,103.78082772182744,824.3066399811788,1228.0361122831844,1632.3046122435817,1433.546233684447,1418.01799566985,963.5434574832974,1763.2038582723696,917.3496123369309,599.2130430603439,1752.0517208998554,1500.8265174531412,917.2578174094253,2003.4937878090898,1330.2055566768338,754.6717134556521,1979.3867895761946,1019.9869142340513,59.66941655516538,1309.4750251513549,23.467038480603602,1777.2538214687218,559.5453560654925,705.090363928449,1069.611606610796,2075.881345998112,851.530495468246,890.828592499109,2256.059648509189,1306.6794103041182,524.7604852513664,159.0452087961126,818.5203263295118,266.62237309369135,608.2420611288375,1365.4364779838847,1586.1151605498908,1444.3802310626002,1492.3204891076757,1359.4853547113203,1986.7603881623377,1093.5777713361413],
    "names": ["Communism","Anti-Leninism","Expansionist nationalism","Republic","National Bolshevism","Radicalism (historical)","Bioconservatism","Religious communism","Khalistan movement","Revisionism (Marxism)","Individualist anarchism in France","Indigenism","Social liberalism","Left-wing politics","Khilafat","Radical right (United States)","Pan-Asianism","Christian democracy","Centre (politics)","Social ecology","Third Way","National Conservatism","Illiberal democracy","Synthesis anarchism","Green municipalism","Clerical fascism","Titoism","Radical right (Europe)","Sankarism","Toryism","One-party state","Crypto-anarchism","Liberal feminism","Anti-fascism","Minority government","Objectivism (Ayn Rand)","Unitary state","Liberal nationalism","Platformism","Radical centrism","Confederation","Strasserism","Pan-Somalism","Semi-democracy","Revolutionary democracy","Social conservatism","Deep ecology","Green politics","Utopian socialism","Christian socialism","Impossiblism","Autonomism","Syncretic politics","Left-wing nationalism","Inclusive Democracy","National-Anarchism","Realism (international relations)","Stateless communism","Green libertarianism","Authoritarianism","Eurocommunism","Anticommunism","Religious nationalism","Right libertarianism","Peronism","Two-party system","National Socialism","Mythology","Communalism","Hindu nationalism","Egoist anarchism","Centrism","Hindutva","Nationalism","Anarchy","Compassionate conservatism","Red Toryism","Illegalism","Labor Zionism","Traditionalist conservatism","Islamic socialism","Marxism","Right-libertarianism","Anarcho-syndicalism","Hegemony","Austrofascism","Federation","Pluralism (political philosophy)","Zapatismo","Christian libertarianism","Paleolibertarianism","Conservatism","Anti-revisionism","Ultra-leftism","Neo-Nazism","Christofascism","Bright green environmentalism","Political Catholicism","Conservatism in the United Kingdom","Cultural feminism","Intellectualism","Chinese nationalism","Totalitarianism","Anocracy","Chiefdom","Kemalism","Usta??e","Kritarchy","Anarcho-capitalism","Progressivism","Brazilian Integralism","Confidence and supply","Womanism","Islamic feminism","Ordoliberalism","Kautskyism","Juche","Zbor","Primitive communism","Capitalism","Autonomist Marxism","Socialism","Arab socialism","Minarchism","Islamic democracy","Christian Right","Dominionism","Anti-Stalinist Left","Castroism","Far-left politics","Anarchist naturism","Neo-Marxism","Mormon feminism","Feminism","Islamofascism","Caesaropapism","Geoanarchism","Cultural liberalism","Buddhist socialism","Marxist revisionism","Insurrectionary anarchism","Melanesian socialism","Classical Marxism","Makhnovism","Green syndicalism","Centre-left politics","Voluntaryism","Symbol","Utilitarianism","Participism","Oligarchy","Military dictatorship","White nationalism","Jewish anarchism","Neo-Fascism","Black conservatism","Hung parliament","Multi-party system","Anarcha-feminism","Psychoanalytic feminism","Italian fascism","Luxemburgism","Christian Zionism","Christian Reconstructionism","Neosocialism","Odalism","Environmentalism","Zionism","Monarchism","Majority government","LGBT social movements","Separatist feminism","Communitarianism","Distributism","Christian feminism","Individualist feminism","Individualist anarchism in the United States","Anti-Revisionism","Pan-Iranism","Constitutional monarchy","Marxist humanism","Democratic socialism","Socialist economics","Lesbian feminism","Social anarchism","New Left","Fabianism","Conservatism in Colombia","Liberation Theology","Romantic nationalism","Islamism","Christian Left","Fanaticism","Political radicalism","Grand coalition","Direct democracy","Queer anarchism","Ultramontanism","Pan-Islamism","Agorism","De Leonism","Non-partisan democracy","Plutocracy","Religious Zionism","Despotism","Falangism","Conservative liberalism","Globalism","Liberal conservatism","Radical feminism","Communization","Far-right politics","Ideal (ethics)","Reactionary","Socialism with Chinese characteristics","Fascism","Guevarism","Social democracy","Mutualism (economic theory)","Dictatorship","Populism","Panislamism","Theodemocracy","Revisionist Zionism","Euroscepticism","Autarchism","Pan-Slavism","Pan-European nationalism","National conservatism","Agrarianism","Syndicalism","State socialism","Moderate","Aristocracy","Gandhian economics","Dominant-party system","Monarchy","Individualist anarchism","Representative democracy","Conservatism in Australia","Economic liberalism","Left communism","Libertarian conservatism","Autocracy","Centre-right politics","Conservatism in Pakistan","Japanese fascism","Nasserism","Civic Conservatism","Pan-Africanism","Diaspora nationalism","Workerism","Iron Guard","Anarcho-naturism","Neoconservatism","Reformism","Islamic anarchism","Eco-capitalism","Queer nationalism","Neo-Zionism","Conservatism in North America","World communism","Fourierism","Conservatism in Canada","Libertarianism","Individualism","Stalinism","Anarchism","Participatory economics","Antifeminism","Black nationalism","National communism","Cultural conservatism","United Order","Roman Catholic conservatism","Neoliberalism","Panarchism","Absolute monarchy","National Socialist Movement of Chile","Coalition government","Inclusive democracy","Green conservatism","Liberalism","Carlism","Masculism","Conservative","Liberal socialism","Collectivist anarchism","Hoxhaism","National Unity government","Baathism","Militarism","Christian communism","Libertarian Marxism","Bolivarianism","Arab nationalism","Popolarismo","Post-anarchism","Extremism","Timocracy","Green anarchism","Theocracy","Eco-socialism","Free-market environmentalism","Paleoconservatism","Post-left anarchy","Anarcho-communism","Anarchism without adjectives","Geolibertarianism","Marxism???Leninism","Socialist feminism","Individualist anarchism in Europe","Theoconservatism","Government","Gaullism","Revolutionary socialism","Orthodox Marxism","Bioregionalism","Libertarian socialism","Christian anarchism","Possibilism (politics)","Conservatism in the United States","Magonism","Patriotism","Quotaism","Austromarxism","Buddhist anarchism","Western Marxism","Bolshevism","Fundamentalism","Atheist feminism","Metaxism","Semi-authoritarian","Ho Chi Minh Thought","Scientific communism","Pan-Celticism","Infoanarchism","Latin Conservatism","Leninism","National liberalism","Pan-Turkism","Integralismo Lusitano","Democracy","Legalism (Chinese philosophy)","Religious feminism","Particracy","Market socialism","Anarchist communism","Republicanism","Paleoliberalism","Trotskyism","African socialism","Conservatism in Germany","Constitutionalism","Coalition","Corporatism","Maoism","Anarcho-primitivism","Market liberalism","Freiwirtschaft","Producerism","Bernsteinism","Marxist feminism","Religious socialism","Ecofeminism","Right-wing politics","Irish Nationalism","Fiscal conservatism","Stratocracy","Colonial liberalism","Irish Republicanism","Council communism","Green liberalism","Guild socialism","Neo-marxism","Postmodern feminism","Internationalism (politics)","Jewish feminism","Islamic Fundamentalism","Scandinavianism","Empire","National syndicalism","Three Principles of the People","Classical liberalism","Industrialism","Situationist International"]
  };
}

function setup() {
  var cw = 1440; //windowWidth;
  var ch = 800; //windowHeight;
  canvas = createCanvas(zoom.x * cw + margin.l + margin.r, zoom.y * ch + margin.t + margin.b);

  textSize(txtSize);  
  
  tl = {x:zoom.x*width, y:zoom.x*height};
  br = {x:-zoom.y*width, y:-zoom.y*height};
  
  var x = data.x;
  var y = data.y;
  var names = data.names;
  
  boxes = [];
  for (var i=0; i<x.length; i++) {
    var x_ = x[i];
    var y_ = y[i];
    if (adjust) { // originals are normalized to {0, 1}, so spread them
      x_ *= (zoom.x * width);
      y_ *= (zoom.y * height);
    }
    var w_ = textWidth(names[i]) + 2 * margin.box;
    var h_ = txtSize + 2 * margin.box;
    var box = {x:x_, y:y_, w:w_, h:h_, txt:names[i]};
    boxes.push(box);
  }
  
  // for unadjusted file, do overlapping procedure (this may take some time)
  if (adjust) {
    var hasOverlap = true;
    while (hasOverlap) {
      hasOverlap = step();
    }
  }
  
  // get bounds
  for (var i=0; i<boxes.length; i++) {
    if (boxes[i].x < tl.x)  tl.x = boxes[i].x;
    if (boxes[i].y < tl.y)  tl.y = boxes[i].y;
    if (boxes[i].x > br.x)  br.x = boxes[i].x;
    if (boxes[i].y > br.y)  br.y = boxes[i].y;
  }
  
  if (adjust) {
    // adjust to (0, 0), and save adusted boxes
    var x_adj = [];
    var y_adj = [];
    for (var i=0; i<boxes.length; i++) {
      boxes[i].x -= tl.x;
      boxes[i].y -= tl.y;
      x_adj.push(boxes[i].x);
      y_adj.push(boxes[i].y);
    }
    br.x -= tl.x;
    br.y -= tl.y;
    tl = {x:0, y:0};
    saveJSON({x:x_adj, y:y_adj, names:names}, filename+"_adjusted.json");  // move this file to your sketch folder
  }
  ox = tl.x - margin.l;
  oy = tl.y - margin.t;
  highlighted = -1;
  infoHighlighted = -1;
  
  // create info box on top left
  var line11 = "Wikipedia articles of ";
  var line12 = "political ideologies";
  var line13 = ",";
  var line21 = "converted to ";
  var line22 = "tf-idf";
  var line23 = ", then clustered by ";
  var line24 = "t-SNE";
  var line25 = ".";
  var line31 = "code: ";
  var line32 = "IPython notebook";
  var line33 = ", visualized with ";
  var line34 = "p5.js";
  var line35 = ".";
  var line41 = "by "
  var line42 = "@genekogan";
  info = [ ];
  info.push({txt:line11, x:tx, y:ty, link:null});
  info.push({txt:line12, x:tx + textWidth(line11), y:ty, link:"https://en.wikipedia.org/wiki/List_of_political_ideologies"});
  info.push({txt:line21, x:tx, y:ty + infoMargin, link:null});
  info.push({txt:line22, x:tx + textWidth(line21), y:ty + infoMargin, link:"https://en.wikipedia.org/wiki/Tf%E2%80%93idf"});
  info.push({txt:line23, x:tx + textWidth(line21) + textWidth(line22), y:ty + infoMargin, link:null});
  info.push({txt:line24, x:tx + textWidth(line21) + textWidth(line22) + textWidth(line23), y:ty + infoMargin, link:"https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding"});
  info.push({txt:line25, x:tx + textWidth(line21) + textWidth(line22) + textWidth(line23) + textWidth(line24), y:ty + infoMargin, link:null});
  info.push({txt:line31, x:tx, y:ty + 2*infoMargin, link:null});
  info.push({txt:line32, x:tx + textWidth(line31), y:ty + 2*infoMargin, link:"https://www.github.com/genekogan/wiki-tSNE"});
  info.push({txt:line33, x:tx + textWidth(line31) + textWidth(line32), y:ty + 2*infoMargin, link:null});
  info.push({txt:line34, x:tx + textWidth(line31) + textWidth(line32) + textWidth(line33), y:ty + 2*infoMargin, link:"https://www.p5js.org"});
  info.push({txt:line35, x:tx + textWidth(line31) + textWidth(line32) + textWidth(line33) + textWidth(line34), y:ty + 2*infoMargin, link:null});
  info.push({txt:line41, x:tx, y:ty + 3*infoMargin, link:null});
  info.push({txt:line42, x:tx + textWidth(line41), y:ty + 3*infoMargin, link:"https://www.twitter.com/genekogan"});

  // draw the screen and turn off frame loop
  drawScreen();
  noLoop();
}

function drawInfoText(txt, x, y, link) {
  push();
  if (link != null) {
    fill(0, 0, 255);
  }
  else {
    fill(0);
  }
  noStroke();
  text(txt, x, y);
  if (link != null) {
    stroke(0, 0, 255, 150);
    strokeWeight(2);
    line(x, y+6, x + textWidth(txt), y+6);
  }
  pop();
}

function drawScreen() {
  background(255);
  
  // draw info box
  push();
  fill(0, 15);
  rect(tx-20, ty-40, 500, 200)
  for (var i=0; i<info.length; i++) {
    drawInfoText(info[i].txt, info[i].x, info[i].y, info[i].link);
  }
  pop();
  
  // draw boxes
  push();
  translate(-ox, -oy);

  for (var i=0; i<boxes.length; i++) {
    //if (boxes[i].x - ox + boxes[i].w < scroll.x || boxes[i].x - ox > scroll.x + ww || boxes[i].y - oy + boxes[i].h < scroll.y || boxes[i].y - oy > scroll.y + wh) {
      //continue;
    //}
    noStroke();
    if (i == highlighted) {
      fill(0, 0, 150);
      text(boxes[i].txt, boxes[i].x + margin.box, boxes[i].y + txtSize + margin.box);
      stroke(0, 0, 255);
      strokeWeight(3);
    }
    else {
      fill(0);
      text(boxes[i].txt, boxes[i].x + margin.box, boxes[i].y + txtSize + margin.box);
      stroke(0, 80);
      strokeWeight(1);
    }
    noFill();
    rect(boxes[i].x, boxes[i].y, boxes[i].w, boxes[i].h);
  }
  pop();
}

function mouseMoved() {
  var mX = mouseX;
  var mY = mouseY+txtSize;
  for (var i=0; i<info.length; i++) {
    if (info[i].link != null) {
      if ((mX > info[i].x) && (mX < info[i].x + textWidth(info[i].txt)) && (mY > info[i].y) && (mY < info[i].y + txtSize + 9)) {
        if (infoHighlighted != i) {
          infoHighlighted = i;
          highlighted = -1;
          canvas.style("cursor", "pointer");
          return;
        }
        else {
          return;
        }
      }
    }
  }
  if (infoHighlighted != -1) {
    infoHighlighted = -1;
    canvas.style("cursor", "default");
    drawScreen();
  }
  mX = mouseX + ox;
  mY = mouseY + oy;
  for (var i=0; i<boxes.length; i++) {
    if ((mX > boxes[i].x) && (mX < boxes[i].x + boxes[i].w) && (mY > boxes[i].y) && (mY < boxes[i].y + boxes[i].h)) {
      if (highlighted != i) {
        highlighted = i;
        canvas.style("cursor", "pointer");
        drawScreen();
      }
      return;
    }
  }
  if (highlighted != -1) {
    highlighted = -1;
    canvas.style("cursor", "default");
    drawScreen();
  }
}

function mousePressed() {
  if (infoHighlighted == -1 && highlighted == -1) {
    return;
  }
  if (infoHighlighted != -1) {
    window.open(info[infoHighlighted].link,'_blank')
  }
  else if (highlighted != -1) {
    window.open('https://en.wikipedia.org/wiki/'+boxes[highlighted].txt,'_blank')
  }
  highlighted = -1;
  infoHighlighted = -1;
  canvas.style("cursor", "default");
  drawScreen();
}

function step() {
  var hasOverlap = false;
  var s = [];
  for (var i=0; i<boxes.length; i++) {
    var s_ = {x:0, y:0};
    for (var j=0; j<boxes.length; j++) {
      if (i==j) continue;
      var o = overlap(boxes[i], boxes[j]);
      if (o > 0) {
        hasOverlap = true;
        var a = atan2(boxes[i].y - boxes[j].y, boxes[i].x - boxes[j].x);
        var d = 1;
        s_.x += d * cos(a);
        s_.y += d * sin(a);
      } 
    }
    s.push(s_);
  }
  // make corrections
  for (var i=0; i<boxes.length; i++) {
    boxes[i].x += s[i].x;
    boxes[i].y += s[i].y;
  }
  return hasOverlap;
}

function overlap(R1, R2) {
  if (R1.x > (R2.x+R2.w) || R2.x > (R1.x+R1.w)) {
    return 0;
  }
  if (R1.y > (R2.y+R2.h) || R2.y > (R1.y+R1.h)) {
    return 0;
  }
  return dist(R1.x+R1.w/2, R1.y+R1.h/2, R2.x+R2.w/2, R2.y+R2.h/2);
}
</script>

<style> 
  body {
    padding: 0; 
    margin: 0;
  }
  #work, .container{
    margin-bottom:0px;
  }
  canvas {
    vertical-align: top;
  }
</style>

{:/nomarkdown} 