<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click="showModalHandler">父子模态窗口</button>
    <button @click="openFileHandler">打开文件目录</button>
    <button @click="addCatalog">创建目录和文件</button>
    <button @click="updateFile">修改文件名称</button>
    <button @click="readWriteFile">读写文件</button>
    <button @click="readWriteFileXML">读取xml文件</button>
  </div>
</template>
<script>
import { ipcRenderer } from "electron";
//node 文件操作
const fs = require("fs");
//node xml 操作
const xml2js = require("xml2js");

//xml 文件路径
const fileXmlPath = "G:\\nodeElectron\\newXml.xml";
//普通文件路径
const path = "G:\\nodeElectron";
export default {
  methods: {
    showModalHandler() {
      ipcRenderer.send("child-down-modal");
    },
    openFileHandler() {
      const { shell } = require("electron").remote;
      //shell.showItemInFolder("G:/text.txt");
      shell.openItem("G://工作文档//技能大赛系统//2号选手.mp4");
      // shell.openItem("G://工作文档//技能大赛系统//新建PPT 演示文稿.ppt");
    },
    //创建文件和目录
    addCatalog() {
      //先创建目录 目录文件操作存在权限问题
      if (!!fs.existsSync(path + "\\aaa")) {
        this.addFiles();
        return console.log("目录：" + path + "\\aaa" + "已存在");
      } else {
        if (!fs.mkdirSync(path + "\\aaa")) {
          this.addFiles();
        } else {
          return console.log("目录：" + path + "\\aaa" + "创建失败");
        }
      }
    },
    addFiles() {
      //创建文件
      fs.writeFile(path + "\\aaa" + "\\test.txt", "hello world!", function(
        err
      ) {
        if (err) {
          return console.log(err);
        }
        console.log("The file was saved!");
      });
    },
    //修改文件名称
    updateFile() {
      let newPath = path + "\\test1.txt";
      fs.rename(path + "\\test.txt", newPath, function(err) {
        if (!err) {
          if (!!fs.existsSync(newPath)) {
            console.log("替换成功");
          } else {
            console.log("替换失败");
          }
        } else {
          console.log("替换失败");
        }
      });
    },
    //读写文件
    readWriteFile() {
      fs.readFile("G:/text.txt", "utf-8", function(err, data) {
        if (err) {
          console.error(err);
        } else {
          console.log(data);
        }
      });
      //fs.readFileSync(fileUrl);
      fs.readFile("G:/text.txt", function(err, data) {
        if (err) {
          console.log(err);
          console.log("err");
        } else {
          console.log("读取第一个文件成功");
          console.log(data.toString());
          fs.readFile("G:/text.txt", "utf-8", function(err, data) {
            if (err) {
              console.log("读取第二个文件失败");
            }
            if (data) {
              console.log("读取第2个文件成功");
              console.log(data);
            }
          });
        }
      });

      // var data = "宋茂林是好人";
      // fs.writeFile(
      //   "G:/text.txt",
      //   data,
      //   { flag: "w", encoding: "utf-8", mode: "0666" },
      //   function(err) {
      //     if (err) {
      //       console.log("文件写入失败");
      //     } else {
      //       console.log("文件写入成功");
      //     }
      //   }
      // );
      var data = "宋茂林是好人-追加";
      fs.writeFile(
        "G:/text.txt",
        data,
        { flag: "a", encoding: "utf-8", mode: "0666" },
        function(err) {
          if (err) {
            console.log("文件写入失败");
          } else {
            console.log("文件追加成功");
          }
        }
      );
    },
    //XML文件读取
    readWriteFileXML() {
      let info = fs.readFileSync(fileXmlPath);
      //xml解析器
      const xmlParser = new xml2js.Parser({ explicitArray: false });
      xmlParser.parseString(info, function(err, result) {
        console.log(result);
        result.CATALOG.PLANT[0].AVAILABILITY = "11111111";

        // /var strings = result.resources;
        //console.log(strings[0]._);
        //将返回的结果再次格式化
        const resultInfo = JSON.stringify(result.CATALOG);
        //console.log(resultInfo);
        const aa = result.CATALOG;
        console.log(aa.PLANT);

        //清空文件中的值
        fs.writeFile(
          fileXmlPath,
          "",
          { flag: "w", encoding: "utf-8", mode: "0666" },
          function(err) {
            if (err) {
              console.log("文件写入1失败");
            } else {
              console.log("文件写入成功");
            }
          }
        );

        //JSON转xml
        var builder = new xml2js.Builder();
        var xml = builder.buildObject(result);

        //写入新的数据
        fs.writeFile(
          fileXmlPath,
          xml,
          { flag: "a", encoding: "utf-8", mode: "0666" },
          function(err) {
            if (err) {
              console.log("文件写入2失败");
            } else {
              console.log("文件追加成功");
            }
          }
        );
      });
    }
  }
};
</script>