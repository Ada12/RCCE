# React Component Category Example

## Overview

This is a graphic interface example about component category we propsed in paper **"Leveraging the Power of Component-based Development for Front-end: Insights from a Study of React Applications"**. The example can help you to understand the usage scenario and code features for different categories.

## Content
As shown below in Figure 1, this is a brake design system that performs a process of generating the optimal parameter combination. Four different components labelled by sequence numbers in Figure 1 respectively are selected by us to illustrate the component categories.

<table class="image">
<caption align="bottom">Figure 1: Graphic Interface Example</caption>
<tr><td><img align="center" height="600" width="570" src ="https://github.com/Ada12/RCCE/raw/master/img/graphic_interface_example.png"/></td></tr>
</table>

##### Decoration Component
Sequence number ① in Figure 1 is a `Decoration Component`, which displays a nice activatable button. Varied interfaces appear by passing in different parameters, and several components ① can compose an `Intent Component`: a step bar. 

Here is a brief snippet of the code for components ①. Some functions and fragments are hidden, and retain only the most explaining part. Among them，the parameters in **'Component.propTypes'** stand for the external interface that this component exposed, by way of passing in parameters, distinct different reused component is produced. These `Decoration Component` provide a better way for software reuse.  A `Decoration Component` is extremely dependent on its father component so that only the **'render'** function exists but lacks of its own state and event handler functions.

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
Sequence number ② in Figure 1 is an `Atom Component`, it has its own state **'value'** to store the filling-in message and handler functions to handle the corresponding event like **'handleInputChange'** and component lifecycle functions like **'componentDidMount'**.

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
As we can see, several component ②, mutex radios and button can be combined into a more complex component, component ③. It is a `Intent Component`. Along with the own states and event handler functions listed below, there are several internal invoked `Atom Components` such as **'MutexRadio'** and **'LabelInput'** and third-party component library like **'Button'** available in the code snippet.

```jsx
import React, { PropTypes as P } from 'react';
import { Button } from 'antd';
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
          <MutexRadio label="Optimization target" options={optionsTarget} onChange={this.handelTargetChange} />
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
Sequence number ④ in Figure 1 represents a kind of `Container Component`. Apart from having the capacities of `Intent Component`, some extra abilities to communicate with external interfaces are the bridge linking up the front-end with back-end. In this example, functions in **'mapDispatches'** are the outgoing requests, and variables in **'DesignContainer.propTypes'** accept external data.

```jsx
import React, { PropTypes as P } from 'react';
import { connect } from 'react-redux';
import { Button, message, Pagination, Table } from 'antd';

// ==================
// Import Components
// ==================

import NavigationBar from '../../components/navigation';
import Steps from '../../components/Steps';
import ParamForm from '../../components/parameter-form';
const Step = Steps.Step;
// ==================
// Actions
// ==================

import DesignActions from '../../actions/design-actions';
import BasicActions from '../../actions/basic-actions';
// ==================
// Map store states to props
// ==================

const mapStoreStateToProps = (state) => ({
  dispatch: state.dispatch,
  similarVehicle: state.design.similarVehicle,
  optimaResult: state.design.optimaResult,
});

const mapDispatches = (dispatch) => ({
  fn: {
    getSimilarVehicle: (obj) => {
      dispatch(DesignActions.getSimilarVehicle(obj));
    },
    getOptimaResult: (obj) => {
      dispatch(DesignActions.getOptimaResult(obj));
    },
    getDownload: (obj) => {
      dispatch(DesignActions.getDownload(obj));
    },
    handleNavClick: (index) => {
      dispatch(BasicActions.getDownload(index));
    },
  },
});

class DesignContainer extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      current: 0,
      paramFormObj: {},
      optimaTarget: {},
    };
  }
  componentDidMount() {}
  componentWillReceiveProps(nextProps) {}
  shouldComponentUpdate(nextProps, nextState) {}
  componentWillUnmount() {}
  
  handleParamSubmit = (obj) => {};
  handleTargetSubmit = (obj) => {};
  handleNext = () => {};
  handlePrev = () => {};
  
  render() {
    const steps = [
      {
        title: 'Parameters',
      }, {
        title: 'Optimization result',
      },
    ];
    const { current } = this.state;
    return (
      <div className="app-container">
        <NavigationBar
              className="navigation-bar"
              current={1}
              onNavClick={this.props.fn.handleNavClick}
            />
        <Steps current={current}>
          {steps.map(item => <Step key={item.title} title={item.title} />)}
        </Steps>
        <div className="step-content">
          {(() => {
            switch (this.state.current) {
            case 0:
              return (
                <ParamForm
                  classname="param-form"
                  onSubmit={this.handleParamSubmit}
                />
              );
              { /* other cases */ }
            }
          })()}
        </div>
        <div className="steps-action">
          { /* other actions */ }
        </div>
      </div>
    );
  }
}

DesignContainer.propTypes = {
  dispatch: P.func,
  fn: P.object,
  location: P.any,
  history: P.any,
  similarVehicle: P.array,
  optimaResult: P.array,
};
// ==================
// Export
// ==================

export default connect(mapStoreStateToProps, mapDispatches)(DesignContainer);
```
