### ant-design 在使用 Select 组件，当 value 为空时显示 placeholder，不为空时，显示默认值？
    <Select placeholder="请选择职位类别" value={jobCategory ? jobCategory : undefined}>
在 Select 组件中，placeholder 显示提示文字， value 控制默认值，有默认值显示默认值，没有默认值显示 placeholder， 如果没有默认值的时候， 写成 undefined 就可以了
