在使用 react 的时候，难免会跟表单元素打交道，在处理表单元素的时候

<input type="text" value={this.state.value} onChange={this.handleChange}></input>

<textarea value={this.state.value} onChange={this.handleChange}></textarea>

 handleChange = (e) => {
    this.setState({
        value: e.target.value
    })
}

如果不想表单元素被 react 约束，那就不用使用 value，而应该使用 defaultValue 
