---
layout: post
title: MongoDB와 GeoJSON을 이용하여 근접 위치 확인하기
category: MongoDB
tags: [MongoDB]
---

## >MongoDB와 GeoJSON을 이용하여 근접 위치 확인하기


<body class="stackedit">
  <div class="stackedit__html">
<p>MongoDB는 NoSQL로 key-value 형태의 BSON(Binary JSON)으로 되어있습니다. ( 스키마가 없어요! )<br>
데이터 저장 형식이 자유로워 MongoDB와 GeoJSON을 이용하면 두 지점간의 거리를 구하거나,<br>
한 지점에서 부터의 일정 반경까지의 거리 계산을 쉽게 할 수 있습니다.</p>
<p>geoJSON이란 위치 정보를 JSON형태로 만든 정보 입니다.<br>
참고 :  <a href="http://geojson.org/">http://geojson.org/</a></p>
<p><strong>GeoJSON</strong> 객체는  <em>{ type: 타입, coordinates: [ 경도, 위도 ] }</em>  형식으로 표현됩니다.<br>
타입에는 Point, LineString, Polygon, MultiPoint, MultiLineString, MultiPolygon, GeometryCollection 등이 있습니다.<br>
Point를 제외하고는  <em>coordinates: [[경도1, 위도1], [경도2, 위도2], …]</em>  이렇게 여러 점을 한 번에 표현할 수 있습니다.<br>
여러 도형을 표현할 수 있는 게 장점입니다.</p>
<p>아래 몽고 쿼리는  -74, 40.74 를 중심으로 반지름이 10인 원 안에 존재하는 위치정보들을 반환합니다.</p>
<pre><code>db.places.find(
   { loc: { $geoWithin: { $center: [ [-74, 40.74], 10 ] } } }
)
</code></pre>
<p>그럼 가장 익숙한 Java/Spring Boot를 이용하여 간단한 위치 기반 API를 만들어 보겠습니다.</p>
<p>Spring Data는 MongoDB- Geospatial를 제공하여 MongoRepository를 상속받아 사용할 수 있습니다.</p>
<p>먼저 MongoDb에 저장할 간단한 LocationEntity를 생성합니다.</p>
<pre><code>@Document(collection = "locations")  
public class LocationEntity {  
    private String id;  
 private String subject;  
 private GeoJsonPoint location;  
  
 public LocationEntity(final String subject, final GeoJsonPoint location) {  
        this.subject = subject;  
 this.location = location;  
  } 
}
</code></pre>
<p>다음으로 LocationEntity에 대한 Repository를 생성합니다.</p>
<pre><code>public interface LocationRepository  extends MongoRepository&lt;LocationEntity, String&gt; {  

    List&lt;LocationEntity&gt; findBySubjectAndLocationNear(String sid, Point p, Distance d);  
}
</code></pre>
<p>마지막으로 RestController를 이용하여 위도, 경도, 거리를 입력받고<br>
입력된 위도, 경도를 중심으로 키로수 반경에 존재하는 위치 정보들을 리턴합니다.</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token annotation punctuation">@RestController</span>
<span class="token keyword">public</span> <span class="token keyword">class</span> <span class="token class-name">LocationResource</span> <span class="token punctuation">{</span>
 <span class="token annotation punctuation">@Autowired</span>
 <span class="token keyword">private</span> LocationRepository repository<span class="token punctuation">;</span>

 <span class="token annotation punctuation">@RequestMapping</span><span class="token punctuation">(</span>method <span class="token operator">=</span> RequestMethod<span class="token punctuation">.</span>GET<span class="token punctuation">)</span>
 <span class="token keyword">public</span> <span class="token keyword">final</span> List<span class="token operator">&lt;</span>LocationEntity<span class="token operator">&gt;</span> <span class="token function">getLocations</span><span class="token punctuation">(</span>
   <span class="token annotation punctuation">@RequestParam</span><span class="token punctuation">(</span><span class="token string">"lat"</span><span class="token punctuation">)</span> String latitude<span class="token punctuation">,</span>
   <span class="token annotation punctuation">@RequestParam</span><span class="token punctuation">(</span><span class="token string">"long"</span><span class="token punctuation">)</span> String longitude<span class="token punctuation">,</span>
   <span class="token annotation punctuation">@RequestParam</span><span class="token punctuation">(</span><span class="token string">"d"</span><span class="token punctuation">)</span> <span class="token keyword">double</span> distance<span class="token punctuation">,</span>
   <span class="token annotation punctuation">@RequestParam</span><span class="token punctuation">(</span>value <span class="token operator">=</span> <span class="token string">"s"</span><span class="token punctuation">,</span> required <span class="token operator">=</span> <span class="token boolean">false</span><span class="token punctuation">)</span> String subjects<span class="token punctuation">)</span> <span class="token punctuation">{</span>

   <span class="token keyword">return</span> <span class="token keyword">this</span><span class="token punctuation">.</span>repository<span class="token punctuation">.</span><span class="token function">findBySubjectAndLocationNear</span><span class="token punctuation">(</span>subjects<span class="token punctuation">,</span>
     <span class="token keyword">new</span> <span class="token class-name">Point</span><span class="token punctuation">(</span>Double<span class="token punctuation">.</span><span class="token function">valueOf</span><span class="token punctuation">(</span>longitude<span class="token punctuation">)</span><span class="token punctuation">,</span> Double<span class="token punctuation">.</span><span class="token function">valueOf</span><span class="token punctuation">(</span>latitude<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
     <span class="token keyword">new</span> <span class="token class-name">Distance</span><span class="token punctuation">(</span>distance<span class="token punctuation">,</span> Metrics<span class="token punctuation">.</span>KILOMETERS<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
 <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<p>이것 만으로도 넘어 오는 GPS정보를 이용하여 근접 지점들을 확인 할 수 있습니다.<br>
엄청 간단하죠!</p>
<p>뭔가 두서없이 썼네요,<br>
잘못된 정보가 있다면 댓글로 알려주세요!</p>
<p>감사합니다.</p>



<h4 id="reference">Reference</h4>
<ul>
<li>
<p><a href="https://docs.mongodb.com/manual/reference/operator/query/near/">https://docs.mongodb.com/manual/reference/operator/query/near/</a></p>
</li>
<li>
<p><a href="https://dzone.com/articles/using-mongodb-geospatial">https://dzone.com/articles/using-mongodb-geospatial</a></p>
</li>
<li>
<p><a href="https://drissamri.be/blog/2015/08/18/build-a-location-api-with-spring-data-mongodb-and-geojson/">https://drissamri.be/blog/2015/08/18/build-a-location-api-with-spring-data-mongodb-and-geojson/</a></p>
</li>
</ul>
<br>
<p>Posted by AN</p>

</div>
</body>
