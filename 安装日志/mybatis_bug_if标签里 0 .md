```xml
<if test=" en_vehicle_class != ''">
    and envc.vc_cd = #{en_vehicle_class,jdbcType=INTEGER}
</if>
```

如果en_vehicle_class的值为0 --**en_vehicle_class != ''**--为真