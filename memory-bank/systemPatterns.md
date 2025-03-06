# System Patterns

## Documentation Structure
- Technology-specific guideline files (e.g., frontend.md, python.md)
- Clear section organization within each file
- Code examples for concrete implementation
- Comprehensive coverage of tools and best practices

## File Organization
```txt
project-requirements/
├── memory-bank/       # Project context and status
│   ├── projectbrief.md
│   ├── techContext.md
│   ├── systemPatterns.md
│   ├── progress.md
│   └── activeContext.md
├── guidelines/        # Technology-specific guidelines
│   ├── frontend/     # Frontend development
│   │   ├── vue.md
│   │   └── assets/  # Frontend resources
│   ├── backend/      # Backend development
│   │   ├── python.md
│   │   ├── postgres.md
│   │   └── assets/  # Backend resources
│   ├── devops/       # DevOps and infrastructure
│   │   ├── kubernetes.md
│   │   └── assets/  # DevOps resources
│   └── shell/        # Shell scripting
│       ├── bash.md
│       └── powershell.md
├── architecture/     # Architecture documentation
│   ├── principles/  # Architecture principles
│   │   ├── general_principles.md
│   │   └── solid_principles.md
│   └── assets/      # Architecture diagrams
└── README.md        # Project overview
```

## Guidelines Pattern
Each technology guideline includes:
- Environment setup
- Code quality tools
- Testing requirements
- Project structure
- Best practices
- Security considerations

## Documentation Principles
```mermaid
mindmap
    root((Documentation))
        Single Source of Truth
            No duplication
            Clear ownership
        Practical Focus
            Code examples
            Real scenarios
        Clear Instructions
            Step by step
            Actionable tasks
        Consistent Format
            Standard templates
            Style guidelines
        Regular Updates
            Review cycles
            Version control
```

## Documentation Lifecycle
```mermaid
flowchart TD
    subgraph Create
        New[New Content] --> Review1[Initial Review]
        Review1 --> Examples[Add Examples]
    end

    subgraph Validate
        Examples --> Tech[Technical Review]
        Tech --> Format[Format Check]
        Format --> Quality[Quality Check]
    end

    subgraph Deploy
        Quality --> Merge[Merge to Main]
        Merge --> Monitor[Monitor & Maintain]
    end

    Monitor -->|Updates Needed| New
```

## Standards and Implementation
```mermaid
flowchart LR
    subgraph Content
        Config[Configuration]
        Setup[Setup Guide]
        Patterns[Patterns]
        Testing[Testing]
    end

    subgraph Process
        Review[Review]
        Verify[Verify]
        Deploy[Deploy]
    end

    Content --> Process
    Process -->|Approved| Docs[Documentation]
    Process -->|Revise| Content
```
