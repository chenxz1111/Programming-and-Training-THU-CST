# Qt复习

#### 头文件

```c++
#include <QInputDialog>
#include <QFileDialog>
#include <QMessageBox>
#include <QKeyEvent>
#include <QTextStream>
#include <QDateTime>
#include <QTimer>
#include <QDebug>
#include <QDir>
#include <QList>
#include <QObject>
#include <QTextCodec>
#include <QPainter>

#include <QLabel>
#include <QLayout>
#include <QPushButton>
//#include <QGraphicsView>
//#include <QtMultimedia/QSound>
//#include <QGraphicsRectItem>
```

#### 对话框

##### 自建对话框

```c++
Dialog dlg(this);
if(dlg.exec()==QDialog::Accepted)
    //do something 
```

##### 打开(保存)文件

```c++
QString curPath=QDir::currentPath();
QString digTitle="Open";
QString filter="all(*.*)";
//打开一个文件
QString FileName = QFileDialog::getOpenFileName(this,digTitle,curPath,filter);
if(!FileName.isEmpty())
	//do something
//打开一堆文件
QStringList FileNames = QFileDialog::getOpenFileNames(this,digTitle,curPath,filter);
for(QString FileName:FileNames)
    //do something

```

##### 输入对话框

```c++
QString dlgTitle="输入对话框";
QString txtLabel="请输入";
bool ok = false；
//输入字符串
QString text = QInputDialog::getText(this,dlgTitle,txtLabel, QLineEdit::Normal,"", &ok);
if (ok && !text.isEmpty())
    //do something
//输入整数
int minValue=0, maxValue=100, stepValue=1; //范围，步长
int inputValue = QInputDialog::getInt(this,dlgTitle,txtLabel,minValue,minValue,maxValue,stepValue,&ok); 
if(ok)
    //do something
```

##### 信息框

```c++
QString dlgTitle="消息框";
QString strInfo="";
QMessageBox::information(this, dlgTitle, strInfo);
QMessageBox::warning(this, dlgTitle, strInfo)
```

#### 计时器

```c++
//类声明
QTimr* tm;
void on_timeout();//槽函数
//构造函数
tm=new QTimer(this);
tm->stop();

connect(tm,&QTimer::timeout,this,&Dialog::on_timeout);
//
tm->setInterval(...);
tm->start();
```

#### 读取文本

```c++
QFile f(FileName);
if(!f.exists()||!f.open(QIODevice::ReadOnly|QIODevice::Text))
    return;
QTextStream stream(&f);
stream.setCodec(QTextCodec::codecForName("GB2312"));	//读取中文
//do something
f.close();
```

#### 读取图片并显示

```c++
//根据文件名读取图片
QImage* tmp=new QImage(FileName);
QPixmap pix = QPixmap(QPixmap::fromImage(*tmp));
//按label的大小保持比例缩放，设置于label中显示
QPixmap dest=pix.scaled(ui->label->size(),Qt::KeepAspectRatio);
ui->label->setPixmap(dest)
```

#### 音效播放

```c++
QT += multimedia

QSound::play(tr(":/sound/....wav"));//filename
```

#### QSignalMapper

```c++
QSignalMapper* m=new QSignalMapper(this);
for(...){
	connect(...,SIGNAL(clicked()),m,SLOT(map()));
   	m->setMapping(p,...);
}
connect(m,SIGNAL(mapped(int)),this,SLOT(...);
```

#### 事件

```c++
void paintEvent(QPaintEvent* ev);
void closeEvent(QCloseEvent * event);
//鼠标事件
void mouseMoveEvent ( QMouseEvent * ev );
void mousePressEvent ( QMouseEvent * ev );
void mouseReleaseEvent ( QMouseEvent * ev );
void mouseDoubleClickEvent( QMouseEvent * ev );
//按键事件
void keyPressEvent(QKeyEvent* ev);
```

#### 时间读取

```c++
QDateTime::QDateTime::currentDateTime().toString("yyyy-MM-dd hh:mm:ss");
//.toUTC() //获得格林尼治标准时
//.addSecs(...)//增加时间
```

#### 随机数生成

```c++
 qsrand(QTime::currentTime().msec());
//随机数初始化，qsrand是线程安全的
qrand(); //获取随机数
```
