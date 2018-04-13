---
title: Form Builder
layout: default
fluid: true
section: builder
weight: 30
lib: builder
---
<div class="row">
  <div class="col-sm-8">
    <h3 class="text-center text-muted">The <a href="https://github.com/formio/formio.js" target="_blank">Form Builder</a> allows you to build a <select class="form-control" id="form-select" style="display: inline-block; width: 150px;"><option value="form">Form</option><option value="wizard">Wizard</option><option value="pdf">PDF</option></select></h3>
    <div class="well" style="background-color: #fdfdfd;">
      <div id="builder"></div>
    </div>
  </div>
  <div class="col-sm-4">
    <h3 class="text-center text-muted">as JSON Schema</h3>
    <div class="well jsonviewer">
      <pre id="json"></pre>
    </div>
  </div>
</div>
<div class="row">
  <div class="col-sm-8 col-sm-offset-2">
    <h3 class="text-center text-muted">which <a href="https://github.com/formio/ngFormio" target="_blank">Renders as a Form</a> in your Application</h3>
    <div id="formio" class="well"></div>
  </div>
  <div class="clearfix"></div>
</div>
<div class="row">
  <div class="col-sm-8 col-sm-offset-2">
    <h3 class="text-center text-muted">which creates a Submission JSON</h3>
    <div class="well jsonviewer">
      <pre id="subjson"></pre>
    </div>
  </div>
  <div class="clearfix"></div>
</div>
<div class="row">
  <div class="col-sm-10 col-sm-offset-1 text-center">
    <h3 class="text-center text-muted">which submits to our API Platform</h3>
    <p>hosted or on-premise</p>
    <a href="https://form.io" target="_blank"><img style="width:100%" src="https://help.form.io/assets/img/formioapi2.png" /></a>
  </div>
</div>
<div class="row" style="margin-top: 40px;">
  <div class="col-sm-12 text-center">
    <a href="https://form.io" target="_blank" class="btn btn-lg btn-success">Get Started</a>
  </div>
</div>
<div class="row well" style="margin-top: 50px;">
  <div class="container">
    <div class="row">
      <div class="col-lg-12 text-center">
        <h2 class="section-heading">We are Open Source!</h2>
        <h3 class="section-subheading text-muted">We are proud to offer our core Form &amp; API platform as Open Source.</h3>
        <h3 class="section-subheading text-muted">Find us on GitHub @ <a href="https://github.com/formio/formio" target="_blank">https://github.com/formio/formio</a></h3>
      </div>
    </div>
    <div class="row">
      <div class="col-md-4"><a href="https://github.com/formio/formio" target="_blank"><img class="img-responsive" src="https://form.io/assets/images/github-logo.png"></a></div>
      <div class="col-md-8">
        <p>Getting started is as easy as...</p>
        <pre style="background-color: white;">git clone https://github.com/formio/formio.git
cd formio
npm install
node server</pre>
      </div>
    </div>
  </div>
</div>
<script type="text/javascript">
var subJSON = document.getElementById('subjson');
var builder = new Formio.Builder(document.getElementById("builder"), {
  display: 'form',
  components: [],
  settings: {
    pdf: {
      "src": "http://localhost:4005/pdf/5acfab476a1276b6173ff01a/file/2adc8d03-cf0f-555f-9e79-099042627618",
      "id": "2adc8d03-cf0f-555f-9e79-099042627618"
    }
  }
}, {
  baseUrl: 'https://examples.form.io'
});

var onForm = function(form) {
  form.on('change', function() {
    subJSON.innerHTML = '';
    subJSON.appendChild(document.createTextNode(JSON.stringify(form.submission, null, 4)));
  });
};

var setDisplay = function(display) {
  builder.setDisplay(display).then(function(instance) {
     var jsonElement = document.getElementById('json');
     var formElement = document.getElementById('formio');
     instance.on('saveComponent', function(event) {
       var schema = instance.schema;
       jsonElement.innerHTML = '';
       formElement.innerHTML = '';
       jsonElement.appendChild(document.createTextNode(JSON.stringify(schema, null, 4)));
       Formio.createForm(formElement, schema).then(onForm);
     });
   
     instance.on('editComponent', function(event) {
       console.log('editComponent', event);
     });
     
     instance.on('updateComponent', function(event) {
       console.log('updateComponent', event);
     });
     
     instance.on('deleteComponent', function(event) {
       console.log('deleteComponent', event);
     });
     
     Formio.createForm(formElement, instance.schema).then(onForm);
   });
};

// Handle the form selection.
var formSelect = document.getElementById('form-select');
formSelect.addEventListener("change", function() {
  setDisplay(this.value);
});

setDisplay('form');
</script>