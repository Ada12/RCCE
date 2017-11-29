# React Component Categorization Example

## Overview

This is a graphic interface example about component categorization we propsed in paper **"Leveraging the Power of Component-based Development for Front-end: Insights from a Study of React Applications"**. The example can help you to understand the usage scenario and code features for different categorizations.

## Content
As shown below in Figure 1, this is a brake design system that performs a process of generating the optimal parameter combination. Four different components labelled by sequence numbers in Figure 1 respectively are selected by us to illustrate the component categorizations.

<table class="image">
<caption align="bottom">Figure 1</caption>
<tr><td><img align="center" height="330" width="440" src ="https://github.com/Ada12/RCCE/raw/master/img/graphic_interface_example.png"/></td></tr>
</table>

##### Decoration Component
Sequence numbers ① in Figure 1 is a `Decoration Component`, which displays a nice activatable button. Varied interface appears by passing in different parameters, and several components ① can compose an `Intent Component`: a step bar. 

Here is a brief snippet of the code for components ①. Some functions and fragments are hidden, retain only the most explaining part. Among them，the parameters in **'Component.propTypes'** stand for the external interface that this component exposed, by way of passing in parameters, distinct different reused component is produced. These `Decoration Component` provide a better way for software reuse. 

```jsx
import React, { PropTypes as P } from 'react';

class StepItem extends React.Component {
  render() {
    return (
      <div className={this.props.className}>
        <div href={this.props.id}>
          <div className="icon-circle">
            <i className={this.props.icon} />
          </div>
          {this.props.desc}
        </div>
      </div>
    );
  }
}

StepItem.propTypes = {
  className: P.string,
  id: P.string.isRequired,
  icon: P.string.isRequired,
  desc: P.string.isRequired,
};
export default StepItem;
```

##### Atom Component
Sequence numbers ② in Figure 1 is an `Atom Component`, it has its own state **'value'** to store the filling-in message and handler functions to handle the corresponding event and component lifecycle.

```jsx
import React, { PropTypes as P } from 'react';

class LabelInput extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: '',
    };
  }
  componentDidMount() {}
  componentWillReceiveProps(nextProps) {}
  getMessage() {}
  handleInputChange(e) {}
  handleInputFocus(e) {}
  handleInputBlur(e) {}
  render() {
    return (
      <div className={this.props.className}>
        <div className="label-input-label">{this.props.label}</div>
        <div className="label-input-input">
          <input
            type="text"
            value={this.state.value}
            onChange={this.handleInputChange}
            onBlur={this.handleInputBlur}
            onFocus={this.handleInputFocus}
          />
        </div>
        <div className="label-input-message">{this.getMessage}</div>
      </div>
    );
  }
}

LabelInput.propTypes = {
  className: P.string,
  label: P.string.isRequired,
  validation: P.string.isRequired,
  onInputComplete: P.func.isRequired,
};

export default LabelInput;

```

##### Intent Component
As we can see, several component ② can be combined into a more complex component, component ③. It is a `Intent Component`, 

```jsx
import React, { PropTypes as P } from 'react';
import MutexRadio from './mutex-radio';
import LabelInput from './label-input';

class ParameterForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      validation: true,
      typeValue: 'LVM',
      GLValue: '',
      CHValue: '',
      FValue: '',
      WLValue: '',
      TSLRValue: '',
    };
  }
  componentDidMount() {}
  componentWillReceiveProps(nextProps) {}
  handleTypeChange = (value) => {};
  handleGLComplete = (value) => {};
  handleCHComplete = (value) => {};
  handleFComplete = (value) => {};
  handleWLComplete = (value) => {};
  handleTSLRComplete = (value) => {};
  handelTargetChange = (value) => {};
  handleSubmit = () => {};
  render() {
    const options = ['LVM', 'GVM'];
    const optionsTarget = ['quality', 'price'];
    const { validation } = this.state;
    return (
      <div className={this.props.className}>
        <div>
          <MutexRadio label="Type" options={options} onChange={this.handleTypeChange} />
          <LabelInput label="GVW/LLVW" validation={validation} onInputComplete={this.handleGLComplete} />
          <LabelInput label="CG Height" validation={validation} onInputComplete={this.handleCHComplete} />
          <LabelInput label="Front %" validation={validation} onInputComplete={this.handleFComplete} />
          <LabelInput label="Wheelbase Length" validation={validation} onInputComplete={this.handleWLComplete} />
          <LabelInput label="Tire Static Loaded Radius" validation={validation} onInputComplete={this.handleTSLRComplete} />
          <MutexRadio label="Type" options={optionsTarget} onChange={this.handelTargetChange} />
        </div>
        <Button type="primary" onClick={this.handleSubmit}/>
      </div>
    );
  }
}

ParameterForm.propTypes = {
  className: P.string,
  onSubmit: P.func.isRequired,
};

export default ParameterForm;

```

##### Container Component
Sequence numbers ④ in Figure 1 is a `Container Component`.
