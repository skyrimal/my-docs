```xml
<resultMap type="cn.ljw.vo.Order" id="OrderMap">
<id column="order_id" jdbcType="INTEGER" property="orderId" />
<result column="order_number" jdbcType="VARCHAR" property="orderNumber" />
<result column="order_time" jdbcType="TIMESTAMP" property="orderTime" />
<collection property="orderDetails" ofType="cn.ljw.vo.OrderDetail" javaType="java.util.List">
	<id column="detail_order_detail_id" jdbcType="INTEGER" property="orderDetailId" />
	<result column="detail_order_id" jdbcType="INTEGER" property="orderId" />
	<result column="detail_commodity_name" jdbcType="VARCHAR" property="commodityName" />
	<result column="detail_commodity_number" jdbcType="INTEGER" property="commodityNumber" />
</collection>
</resultMap>
```