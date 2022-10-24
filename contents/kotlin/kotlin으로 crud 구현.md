# kotlin으로 개발하기

> kotlin, springboot로 crud 구현해보기!
>
> 

<br>

### - Entity

```kotlin
@Entity
class Board (
 
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    var id: Long? = null,
    var writer: String, 
    var password: String,
    ){
	
    // 수정
    fun updatePost(boardFormDto: BoardFormDto){
        writer = boardFormDto.writer
        password = boardFormDto.password
    }

}
```

<br>

### - Reposiotry

```kotlin
@Repository
interface BoardRepository: JpaRepository<Board, Long> {
}
```

<br>

### - DTO

```kotlin
data class BoardFormDto (
    var writer: String,
    var password: String,
    var title: String,
    var content: String
        ){
}
```

<br>

### - Service

```kotlin
@Service
class BoardService @Autowired constructor(
    val boardRepository: BoardRepository,
    val modelMapper: ModelMapper
) {

    fun save(boardFormDto: BoardFormDto): Long? {
        return boardRepository.save(modelMapper.map(boardFormDto, Board::class.java)).id
    }

    fun getPost(id: Long): Board? {
        return boardRepository.findById(id).get()
    }

    fun deletePost(id: Long) {
        boardRepository.deleteById(id)
    }

    fun updatePost(id: Long, boardFormDto: BoardFormDto): Board {
        val post = boardRepository.findById(id).get()
        post.updatePost(boardFormDto)
        return post
    }

    fun getPostList(): List<Board> {
        return boardRepository.findAll()
    }

}
```

<br>

### - Controller

```kotlin
@RestController //REST API
@RequestMapping("board")
class BoardController @Autowired constructor(val boardService: BoardService) {

	//게시글 등록
    @PostMapping
    fun addPost(boardFormDto: BoardFormDto): ResponseEntity<Any> {
        val save = boardService.save(boardFormDto)
        return ResponseEntity.ok().body(save)
    }
	
    //게시글 읽기
    @GetMapping("/{id}")
    fun getPost(@PathVariable id: Long): ResponseEntity<Any> {
        val post = boardService.getPost(id)
        return ResponseEntity.ok().body(post)
    }

	//게시글 삭제
    @DeleteMapping("/{id}")
    fun deletePost(@PathVariable id: Long): ResponseEntity<Any> {
        boardService.deletePost(id)
        return ResponseEntity.ok().body(true)
    }

	//게시글 수정
    @PutMapping("/{id}")
    fun updatePost(
        @PathVariable id: Long,
        boardFormDto: BoardFormDto
    ): ResponseEntity<Any> {
        val post = boardService.updatePost(id, boardFormDto)
        return ResponseEntity.ok().body(post)

    }

	//게시글 목록
    @GetMapping("/list")
    fun listPost(): ResponseEntity<Any> {
        return ResponseEntity.ok().body(boardService.getPostList())
    }

}
```







