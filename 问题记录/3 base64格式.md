#base64

```java
package gsboot.utils.yangkejiutil.base64;

/**
 * @Description: TODO
 * @author: skyrimal
 * @date: 2020年11月03日 11:54
 */
public class BASE64_PRE {
    public static final String DATA = "data:";//文本数据
    public static final String TEXT_DATA = DATA+"text/plain,";//文本数据

    public static final String HTML_DATA = DATA+"text/html,";//HTML代码
    public static final String HTML_BASE64_DATA = DATA+"text/html;base64,";//base64编码的HTML代码

    public static final String CSS_DATA = DATA+"text/css,";//CSS代码
    public static final String CSS_BASE64_DATA = DATA+"text/css;base64,";//base64编码的CSS代码

    public static final String JS_DATA = DATA+"text/javascript,";//Javascript代码
    public static final String JS_BASE64_DATA = DATA+"text/javascript;base64,";//Javascript代码


    public static final String GIF_DATA = DATA+"image/gif;base64,";//base64编码的gif图片数据
    public static final String PNG_DATA = DATA+"image/png;base64,";//base64编码的png图片数据
    public static final String JPEG_DATA = DATA+"image/jpeg;base64,";//base64编码的jpeg图片数据
    public static final String JPG_DATA = DATA+"image/jpg;base64,";//base64编码的jpg图片数据
    public static final String X_ICON_DATA = DATA+"image/x-icon;base64,";//base64编码的icon图片数据


    public static String matchBASE64_PRE(String fileName){
        String fileExtName = fileName.substring(fileName.indexOf(".")+1);
        return matchBASE64_PREByFileExtName(fileExtName);
    }
    public static String matchBASE64_PREByFileExtName(String fileExtName){
        switch(fileExtName){
            case "x-icon":
                return X_ICON_DATA;
            case "jpg":
                return JPG_DATA;
            case "jpeg":
                return JPEG_DATA;
            case "png":
                return PNG_DATA;
            case "gif":
                return GIF_DATA;
            case "js":
            case "javascript":
                return JS_BASE64_DATA;
            case "css":
                return CSS_BASE64_DATA;
            case "html":
                return HTML_BASE64_DATA;
            default:
                return DATA+",";
        }
    }
    /**
     *  data:,文本数据  data:text/plain,文本数据 data:text/html,HTML代码
     *
     *  data:text/html;base64,base64编码的HTML代码
     *
     *  data:text/css,CSS代码 data:text/css;base64,base64编码的CSS代码
     *
     *  data:text/javascript,Javascript代码 data:text/javascript;base64,base64编码的Javascript代码
     *
     *  data:image/gif;base64,base64编码的gif图片数据 data:image/png;base64,base64编码的png图片数据
     *
     *  data:image/jpeg;base64,base64编码的jpeg图片数据
     *
     *  data:image/x-icon;base64,base64编码的icon图片数据
     */
}
```