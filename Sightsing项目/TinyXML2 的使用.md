# TinyXML2 使用

#### 1. 输出 `xml` 文件

| XMLDocument doc                          | 文本类，包含元素  |
| ---------------------------------------- | ----------------- |
| XMLElement                               | 元素类，如 <root> |
| XMLElement::SetAttribute(name, value)    | 设置属性          |
| XMLElement::InsertEndChild(XMLElement e) | 插入元素          |
| XMLDocument::Parse                       | 解析字符串        |
| XMLDocument::SaveFile(const char *)      | 保存当前文本      |

```C++
int createXML(const char* xmlPath)
{
    const char* declaration ="<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"no\"?>";
    XMLDocument doc;
    doc.Parse(declaration);//会覆盖xml所有内容
    XMLElement* root = doc.NewElement("DBUSER");
    XMLElement* user = doc.NewElement("USER");
    user->SetAttribute("Name", "yangweiming");
    user->SetAttribute("Age", "20");
    user->SetAttribute("height", "160");
    root->InsertEndChild(user);
    doc.InsertEndChild(root);

    return doc.SaveFile(xmlPath);
}
```



#### 2. 遍历元素

| XMLDocument::RootElement                    | 取根结点元素                   |
| ------------------------------------------- | ------------------------------ |
| XMLElement::FirstChildElement               | 取第一个元素                   |
| XMLElement::NextSiblingElement              | 取当前元素的下一个元素         |
| XMLElement::Value()                         |                                |
| const char *XMLElement::GetText()           | 获取当前元素包含的内容         |
| XMLAttribute * XMLElement::FirstAttribute() | 取当前元素的第一个属性         |
| XMLAttribute *XMLAttribute::Next()          | 取当前元素当前属性的下一个属性 |
| XMLAttribute::Name()                        |                                |
| XMLAttribute::Value()                       |                                |

```C++
int loadXML(const char* xmlPath){
    XMLDocument doc;
    doc.LoadFile(xmlPath);
    XMLElement *root=doc.RootElement();
    XMLElement *user = root->FirstChildElement( "USER" );
    while (user)
    {
        XMLElement *surfaceChild=user->FirstChildElement();
        const char* content;
        const XMLAttribute *attributeOfSurface = user->FirstAttribute();
        while(attributeOfSurface){
            cout<< attributeOfSurface->Name() << ":" << attributeOfSurface->Value() << endl;
            attributeOfSurface = attributeOfSurface->Next();
        }
        while(surfaceChild)
        {
            content=surfaceChild->GetText();
            surfaceChild=surfaceChild->NextSiblingElement();
            cout<<content<<endl;
        }
        user=user->NextSiblingElement();
    }
}
```

