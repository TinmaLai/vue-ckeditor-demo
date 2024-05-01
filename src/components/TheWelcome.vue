<template>
    <ckeditor ref="refckeditor" :editor="editor" v-model="editorData" :config="editorConfig" @input="onChange"></ckeditor>
  <button @click="getvalue">getdata</button>
</template>
<script>
import { defineComponent, onMounted, ref, getCurrentInstance, computed } from 'vue'
// import 'ckeditor5-custom-build/build/ckeditor'
// import { ClassicEditor } from '@ckeditor/ckeditor5-editor-classic'
// import { FontFamily, FontSize, FontColor } from '@ckeditor/ckeditor5-font'
// import {Bold, Italic, Underline} from '@ckeditor/ckeditor5-basic-styles';
// import {Alignment} from '@ckeditor/ckeditor5-alignment';
import 'ckeditor5-custom-build/build/ckeditor';
export default defineComponent({
  name: 'TheWelcome',
  setup() {
    const editorInstance = ref({})
    const { proxy } = getCurrentInstance()
    var editor = ClassicEditor
    var editorData = '<p>Content of the editor.</p>';
    var editorConfig = {
      // plugins: [FontFamily, FontSize, FontColor, Bold, Italic, Underline, Alignment],
      fontSize: {
            options: [
                '8px',
                '10px',
                '12px',
                '14px',
                '16px',
                '18px',
                '20px',
                '22px',
                '24px'
            ]
        },
      toolbar: {
        items: ['fontFamily', 'fontSize', 'fontColor', 'bold', 'italic', 'underline', 'alignment']
      }
    }
    onMounted(() => {
      setTimeout(() => {
      // Ẩn biểu tượng.
      document.querySelector('.ck.ck-dropdown.ck-font-family-dropdown').children[0].children[0].style.display = 'none';
      document.querySelector('.ck.ck-dropdown.ck-font-size-dropdown').children[0].children[0].style.display = 'none';
      setDisplayFontFamily('Times New Roman');
      setDisplayFontSize('16px');
      // Get the fontFamily command
      const fontFamilyCommand = proxy.$refs.refckeditor.instance.commands.get('fontFamily');
      const fontSizeCommand = proxy.$refs.refckeditor.instance.commands.get('fontSize');
      // Override the execute event
      fontFamilyCommand.on('execute', (eventInfo, args) => {
        // Prevent the original event from being executed
        eventInfo.stop();
        if(args[0].value){
          setDisplayFontFamily(args[0].value?.split(',')[0].replace(`'`,''))
        } else {
          setDisplayFontFamily('Arial');
        }

        // Execute the command with your own logic
        // This will set the font family to the chosen value without executing the original event
        // Log the chosen font family
        console.log(`Font family chosen: ${args[0].value}`);
      });
      fontSizeCommand.on('execute', (eventInfo, args) => {
        // Prevent the original event from being executed
        debugger;
        eventInfo.stop();
        if(args[0].value){
          setDisplayFontSize(args[0].value?.split(',')[0].replace(`'`,''))
        } else {
          setDisplayFontSize('Arial');
        }

        // Execute the command with your own logic
        // This will set the font family to the chosen value without executing the original event
        // Log the chosen font family
        console.log(`Font family chosen: ${args[0].value}`);
      });
      proxy.$refs.refckeditor.instance.model.document.selection.on('change:range', () => {
        setTimeout(() => {
          var currentFontFamily = proxy.$refs.refckeditor.instance.commands.get('fontFamily').value;
          if(currentFontFamily){
            setDisplayFontFamily(currentFontFamily?.split(',')[0].replace(`'`,''))
          } else {
            setDisplayFontFamily('Arial');
          }
          console.log(currentFontFamily);
        }, 0);
      })
      }, 300);
    })
    function getvalue() {
      debugger
    }
    // const fontFamily = computed(() => {
    //   const fontFamily = proxy.$refs.refckeditor.instance.commands.get('fontFamily').value.split(',')[0];
    //   return fontFamily ? fontFamily : null;
    // });
    function onChange(e){debugger;

    }
    function setDisplayFontFamily(font){
      document.querySelector('.ck.ck-dropdown.ck-font-family-dropdown').children[0].children[1].style.display = 'block';
      
      document.querySelector('.ck.ck-dropdown.ck-font-family-dropdown').children[0].children[1].innerText = font;
    }
    function setDisplayFontSize(fontSize){
      document.querySelector('.ck.ck-dropdown.ck-font-size-dropdown').children[0].children[1].style.display = 'block';
      
      document.querySelector('.ck.ck-dropdown.ck-font-size-dropdown').children[0].children[1].innerText = fontSize;
    }
    return {
      editor,
      editorData,
      editorConfig,
      getvalue,
      editorInstance,
      onChange,
      // fontFamily
    }
  },
})
</script>
<style lang="scss">
.ck.ck-dropdown.ck-font-family-dropdown{
  button.ck.ck-button.ck-off.ck-dropdown__button{
    svg.ck.ck-icon.ck-reset_all-excluded.ck-icon_inherit-color.ck-button__icon{
      display: none;
    }
  }
}
</style>