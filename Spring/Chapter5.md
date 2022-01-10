## **2022/01/10 TIL**

```java
public class UserTokenService extends UserInfoTokenServices {

    public UserTokenService(ClientResources resources, SocialType socialType) {
        super(resources.getResource().getUserInfoUri(), resources.getClient().getClientId());
        setAuthoritiesExtractor(new OAuth2AuthoritiesExtractor(socialType));
    }

    public static class OAuth2AuthoritiesExtractor implements AuthoritiesExtractor {

        private String socialType;

        public OAuth2AuthoritiesExtractor(SocialType socialType) {
            this.socialType = socialType.getRoleType();
        }

        @Override
        public List<GrantedAuthority> extractAuthorities(Map<String, Object> map) {
            return AuthorityUtils.createAuthorityList(this.socialType);
        }
    }

}
```
- UserTokenSercice클래스는 UserInfoTokenService를 상속받는 클래스

- UserInfoTokenService클래스는 스프링 시큐리티 OAuth2에서 제공하는 클래스인데 1.5에는 존재하고 2.0에는 존재하지 않음

- UserInfoTokenService클래스는 User의 정보를 얻어오기 위해 소셜 서버와 통신하는 역할을 수행하는데 이때 URI와 clientId정보가 필요

    - 이때 URI는 userInfoUri로 사용자의 개인정보 조회를 위한 URI
    - clientId는 OAuth클라이언트 사용자명으로 OAuth공급자가 클라이언트를 식별하는 데 사용하는 Id



```java
@Controller
public class LoginController {

    @GetMapping("/login")
    public String login(){
        return "login";
    }
    
    @GetMapping(value="/{facebook|google|kakao}/complete")
    public String loginComplete(HttpSession session){
        OAuth2Authentication authentication=(OAuth2Authentication) 
                SecurityContextHolder.getContext().getAuthentication();
        Map<String,String> map=(HashMap<String,String>)
                authentication.getUserAuthentication().getDetails();
        session.setAttribute("user",User.builder()
                .name(map.get("name"))
                .email(map.get("email"))
                .principal(map.get("id"))
                .socialType(SocialType.FACEBOOK)
                .createdDate(LocalDateTime.now())
                .build()
        );
        return "redirect:/board/list";
    }
    
}

```
- loginController는 인증된 User의 정보를 불러오는 기능을 가지고 있습니다.

- loginController에서는 인증이 성공적으로 처리된 이후에 URI를 리다이렉트해주면 로그인한 유저의 값을 세션에 저장

- SecurityContextHolder에서 인증된 정보를 OAuth2Autentication형태로 받아옴, OAuth2Autentication은 기본적인 인증에 대한 정보뿐만 아니라 OAuth2인증과 관련된 정보도 함께 제공

- 위의 컨트롤러는 각각의 소셜별로 인증받은 User를 처리하는 로직이 다르기 때문에 소셜별로 로직을 추가해줘야함
