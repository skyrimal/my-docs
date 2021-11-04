```java
/**
 * 下载文件
 * @param request
 * @param response
 */
@GetMapping("/download/{fileName}")
public void download(@PathVariable String fileName, HttpServletRequest request, HttpServletResponse response) {
    String path="D:\\file";
    //下载
    try (InputStream inputStream = new FileInputStream(new File(path, fileName ));
         OutputStream outputStream = response.getOutputStream();) {
        response.setContentType("application/x-download");
        response.addHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(fileName, "utf-8"));//关键，不然读不出文件名
        IOUtils.copy(inputStream, outputStream);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

