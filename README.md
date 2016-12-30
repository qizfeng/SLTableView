# SLTableView

SLTableView是一个基于RecyclerView的分组显示库. SLTableView 旨在减少代码量, 增加UI的复用率, 通过简单的配置, 从而实现各种分组效果.

<!--###操作效果-->
<!--![Alt Text](https://github.com/xiaoshanlin000/SLTableView/raw/master/screen/demo.gif)-->

###效果图
<img width="384" height="640" src="https://github.com/xiaoshanlin000/SLTableView/raw/master/screen/1.png"/> <img width="384" height="640" src="https://github.com/xiaoshanlin000/SLTableView/raw/master/screen/2.png"/>
<img width="384" height="640" src="https://github.com/xiaoshanlin000/SLTableView/raw/master/screen/3.png"/> <img width="384" height="640" src="https://github.com/xiaoshanlin000/SLTableView/raw/master/screen/4.png"/>

#快速开始

###导入库
maven
```Java
<dependency>
  <groupId>com.shanlin.library.sltableview</groupId>
  <artifactId>library</artifactId>
  <version>1.0.4</version>
  <type>pom</type>
</dependency>
```

gradle
```Java
compile 'com.shanlin.library.sltableview:library:1.0.4'
```

lvy
```Java
<dependency org='com.shanlin.library.sltableview' name='library' rev='1.0.4'>
  <artifact name='library' ext='pom' ></artifact>
</dependency>
```

###使用介绍

必须实现的接口
```Java
SLTableViewDataSource (配置数据信息)
```
可选接口
```Java
SLTableViewDataSourcePlus(配置header,floor信息)
SLTableViewSpanSizeLookup(配合GridLayoutManager,设置一列的跨度)
SLTableViewCell.SLCellViewClickListener (cell内某view的点击监听接口)

```
初始化SLTableView
```Java
    tableView = new SLTableView.Builder(context)
                .setTableViewDataSource(this)
                .setTableViewDataSourcePlus(this)
                .showStickyHeader(false)
                .build();
    view.addView(tableView);
```
demo
```Java
   @Override
    public int numberOfSections(SLTableView tableView) {
        return dataLists.size();
    }

    @Override
    public int numberOfRowsInSection(SLTableView tableView, int section) {
        return dataLists.get(section).size();
    }

    @Override
    public int typeOfIndexPath(SLTableView tableView, SLIndexPath indexPath) {
        int section = indexPath.getSection();
        if (section < dataLists.size() - 1) {
            return indexPath.getSection() % 2;
        }
        return 2;
    }

    @Override
    public SLTableViewCell cellForType(SLTableView tableView, ViewGroup parent, int type) {
        SLTableViewCell cell = null;
        if (type == 0) {
            View rootView = inflater.inflate(R.layout.type_one_cell, parent, false);
            cell = new TypeOneCell(rootView);
        }else if(type == 1){
            View rootView = inflater.inflate(R.layout.type_two_cell, parent, false);
            cell = new TypeTwoCell(rootView);
        }else if(type == 2){
            View rootView = inflater.inflate(R.layout.type_three_cell, parent, false);
            cell = new TypeThreeCell(rootView);
        }
        return cell;
    }

    @Override
    public void onBindCell(SLTableView tableView, SLTableViewCell cell, SLIndexPath indexPath, int type) {

        final int section = indexPath.getSection();
        final int row = indexPath.getRow();
        if (type == 0){
            TypeOneCell typeCell = (TypeOneCell) cell;
            typeCell.cell_textView.setText(dataLists.get(section).get(row));
            typeCell.bindCellViewClick(typeCell.cell_layout,this);
        }else if (type == 1){
            TypeTwoCell typeCell = (TypeTwoCell) cell;
            typeCell.cell_textView.setText(dataLists.get(section).get(row));
            typeCell.bindCellViewClick(typeCell.cell_layout,this);
        }else if (type == 2){
            final TypeThreeCell typeCell = (TypeThreeCell) cell;
            typeCell.cell_textView.setText(dataLists.get(section).get(row));
            typeCell.bindCellViewClick(typeCell.cell_layout,this,typeCell.cell_textView);
        }

    }
```


###License
```Java
Copyright shanlin

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```