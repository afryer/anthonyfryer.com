<template>
  <div class="blog wrapper" id="blog">
    <Spinner1/>
    <div class="interface">
      <BlogHeader/>
      <main class="main" role="main">
        <nuxt/>
      </main>
      <BlogFooter/>
    </div>
  </div>
</template>

<script>
import BlogHeader from '../components/Header'
import BlogFooter from '../components/Footer'
import Spinner1 from '~/components/Spinner1'

export default {
  components: {
    BlogHeader,
    BlogFooter,
    Spinner1
  }
}
</script>

<style lang="scss">
@import '~assets/css/vars.scss';



html {
  background-color: #f5f5f5;

  font-kerning: normal;

  transition: background-color 0.8s;
  transition-timing-function: cubic-bezier(.11,.89,.31,.99);

  @media (max-width: 500px) {
    font-size: 18px;
  }
}



#blog {
  > .interface {
    opacity: 0;
    overflow: hidden;

    @media (max-width: 700px) {
    }
  }

  > .spinner {
    display: block;
    left: calc(50% - 20px);
    position: absolute;
    top: calc(50vh - 20px - 64px);
    z-index: 10;
  }
}

html.wf-active #blog {
  > .interface  {
    transition: 0.5s;
    opacity: 1;
  }

  > .spinner {
    display: none;
  }
}

// main {
//   background-color: #efefef;
//   margin: 0 auto;
//   max-width: $containerWidth;
//   min-height: calc(100vh - 80px - 200px);
//   width: 100%;

//   @media (max-width: 700px) {
//     min-height: calc(100vh - 60px - 200px);
//   }
// }

.page-enter-active {
  transition: opacity .2s;
}

.page-enter, .page-leave-active {
  opacity: 0;
}

// .page-title {
//   border-bottom: 1px dotted lighten($primary, 20%);
//   margin-top: 32px;
//   margin-bottom: 32px;

//   h1 {
//     font-family: 'Roboto', sans-serif;
//     font-size: 3rem;
//     font-weight: 100;
//     line-height: 1;
//     margin-bottom: 12px;
//     margin-top: 0;
//     padding-bottom: 0;

//     @media (max-width: 500px) {
//       font-size: 2rem;
//     }
//   }
// }

// h1, h2, h3, h4, h5 {
//   color: #333;
//   font-weight: 400;
// }

// a {
//   color: $primary;
//   font-weight: 400;
//   position: relative;
//   text-decoration: none;
//   transition: 0.1s;
// }

// a.fancy {
//   &:hover {
//     color: $accent;
//   }

//   &::after {
//     background: rgba($accent, 0.5);
//     content: '';
//     height: 1px;
//     left: 0;
//     opacity: 0;
//     position: absolute;
//     top: 100%;
//     transform: translateY(-4px);
//     transition: height 0.1s, opacity 0.1s, transform 0.1s;
//     width: 100%;
//   }

//   &:hover,
//   &:focus {
//     &::after {
//       height: 2px;
//       opacity: 1;
//       transform: translateY(0px);
//     }
//   }
// }

// pre {
//   white-space: pre-wrap;
// }

.lazy {

  div:not(.image-height),
  img {
    backface-visibility: hidden;
    height: 100%;
    opacity: 0;
    transition: opacity 0.5s;
    width: 100%;

    &[lazy=loaded] {
      opacity: 1;
    }
  }
}
// }
</style>
