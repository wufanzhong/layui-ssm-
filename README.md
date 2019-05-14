# layui+ssm实现动态下拉框
# 目前遇到了问题，希望有高手能帮忙解决一下
# 1. JSP内容
<form class="layui-form" action="">

       
        <div class="layui-form-item">
            <label class="layui-form-label">选择场地</label>
            <div class="layui-input-block">
                <select name="selectId" id="interes" lay-filter="spceinfo">
                    <option value=""></option>
                </select>
            </div>
        </div>
</form>



# 2. js部分
    layui.use(['form', 'layedit', 'laydate', 'layer','jquery', 'upload'], function(){
        var form = layui.form
            ,layer = layui.layer
            ,layedit = layui.layedit
            ,laydate = layui.laydate;
        //日期
        laydate.render({
            elem: '#date'
        });
        laydate.render({
            elem: '#bookDate'
        });
        //动态添加下拉框     同时可以设置默认值
        $.ajax({
            url: '<%=path%>/space/getSpaceInfoList',
            dataType: 'json',
            type: 'get',
            success: function (data) {
                $.each(data, function (index, item) {
                    // language=JQuery-CSS
                    $('#selectId').append(new Option(item.sname,item.snum));// 下拉菜单里添加元素
                });
                layui.form.render("select");//重新渲染 固定写法
            }
        });
    });
 


# 3. 后端Controller

    @Resource
    private SpaceInfoService spaceInfoService;

    private SpaceInfo jsonData(String data){
        JSONObject jsonObject = JSON.parseObject(data);
        int snum =  jsonObject.getInteger("snum");
        String sname = jsonObject.getString("sname");
        SpaceInfo spaceInfo = new SpaceInfo(snum, sname);
        return spaceInfo;
    }

    //下拉框
    @RequestMapping("/getSpaceInfoList")
    public @ResponseBody
    Map<String, Object> getSpaceInfoList(){
        List<SpaceInfo> countData = spaceInfoService.findAll();
        Map<String,Object> resultMap = new HashMap<>();
        List<SpaceInfo> members =spaceInfoService.findAll();
        resultMap.put("code",0);
        resultMap.put("msg","");
        resultMap.put("count",countData.size());
        resultMap.put("data",members);
        return resultMap;
    }
