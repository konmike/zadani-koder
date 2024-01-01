<script>
import { DatePicker } from 'element-ui'

export default {
  extends: DatePicker,
  props: {
    pickerOptions: {
      type: Object,
      default () {
        return {
          firstDayOfWeek: 1
        }
      }
    }
  }
}
</script>

<style lang="scss">
.el-date-editor,
.el-date-editor.el-input {
  width: 100%;
  max-width: 100%;
  margin: 0;
}
.el-date-editor .el-input__prefix {
  display: none;
}
.el-date-editor .el-input__suffix {
  display: flex;
  align-items: center;
  right: 15px;

  &-inner {
    .el-input__icon {
      line-height: 1;
      &::after {
        background-image: url('/assets/calendar-icon.svg');
        background-position: center;
        background-repeat: no-repeat;
        background-size: cover;
        height: 24px;
        width: 24px;
      }
    }
  }
}
</style>
