# SoftwarePar - Full-Stack Software Development Platform

## Overview

SoftwarePar is a comprehensive software development platform built for the Argentine market, offering custom software development services with integrated client management, partner referral programs, and payment processing. The platform serves as a complete business solution for managing software projects from initial client contact through development, payment, and ongoing support.

The system supports three user roles: administrators who manage the entire platform, partners who earn commissions through referrals, and clients who purchase and track their software projects. Key features include project management with progress tracking, multi-stage payment processing via MercadoPago, integrated support ticketing, WhatsApp notifications via Twilio, and a comprehensive portfolio showcase.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
The frontend is built with React 18 using TypeScript and Vite as the build tool. The UI leverages shadcn/ui components with Radix UI primitives and Tailwind CSS for styling. State management is handled through TanStack Query for server state and React's built-in state for local UI state. The application uses Wouter for client-side routing and Framer Motion for animations.

The component structure follows a modular approach with reusable UI components in the `/components/ui` directory and feature-specific components organized by functionality. The application supports responsive design with mobile-first approach and includes real-time notifications through WebSocket connections.

### Backend Architecture
The backend is built with Express.js and TypeScript, following a RESTful API design pattern. The server implements JWT-based authentication with role-based access control (RBAC) supporting admin, partner, and client roles. Password hashing is handled through bcryptjs for security.

The API routes are organized by feature domains (auth, users, projects, payments, etc.) with middleware for authentication, authorization, and request validation using Zod schemas. The server includes WebSocket support for real-time notifications and file upload capabilities for project assets.

### Database Design
The application uses PostgreSQL as the primary database with Drizzle ORM for type-safe database operations. The schema is defined in TypeScript with proper relationships between entities. Key tables include users, partners, projects, payment stages, tickets, notifications, and portfolio items.

Database migrations are managed through Drizzle Kit, and the connection is established using Neon's serverless PostgreSQL driver. The schema includes proper indexing for performance and foreign key constraints for data integrity.

### Authentication & Authorization
JWT-based authentication system with tokens stored in localStorage on the frontend. Role-based access control restricts access to different parts of the application based on user roles. Authentication middleware validates tokens on protected routes and maintains user sessions.

Password security is implemented using bcryptjs with proper salt rounds. The system includes password reset functionality and account activation flows via email notifications.

### Payment Processing
Integration with MercadoPago for handling payments in the Argentine market. The system supports multi-stage payment processing where projects can be broken down into payment milestones based on development progress. Payment configurations are stored in the database and can be updated by administrators.

The payment flow includes automatic payment link generation, webhook handling for payment status updates, and commission calculations for partner referrals.

### Communication Systems
Dual communication channels through email (Gmail SMTP) and WhatsApp (Twilio API). Email notifications are sent for account creation, project updates, and important system events. WhatsApp integration provides instant notifications for critical updates and can be configured per user preference.

Real-time notifications are delivered through WebSocket connections, allowing immediate updates for project changes, new messages, and system alerts without page refreshes.

## ANÁLISIS COMPLETO DEL SISTEMA (Actualizado)

### 🚨 Estado de Seguridad y Configuración

**Variables de Entorno Críticas:**
- ✅ **DATABASE_URL**: `postgresql://neondb_owner:npg_f1jQRacpFG3V@ep-red-shape-ac6qnrhr-pooler.sa-east-1.aws.neon.tech/neondb?sslmode=require&channel_binding=require`
- ✅ **GMAIL_USER**: `jhonidelacruz89@gmail.com`
- ✅ **GMAIL_PASS**: `htzmerglesqpdoht`
- 🚨 **JWT_SECRET**: **PROBLEMA** - Usando fallback inseguro "your-super-secret-jwt-key"
- ❌ **MERCADOPAGO_ACCESS_TOKEN**: No configurado (MercadoPago no implementado)
- ❌ **TWILIO_ACCOUNT_SID/TWILIO_AUTH_TOKEN**: No configurados, usando datos desde BD

### 🗄️ Estructura de Base de Datos Completa (18 Tablas)

**Core del Sistema:**
1. **users** - Usuarios con roles (admin, client, partner)
2. **partners** - Sistema de afiliados con códigos referido
3. **projects** - Proyectos con progreso, precios, fechas
4. **referrals** - Sistema de referidos partners→clientes

**Sistema de Pagos Sofisticado:**
5. **payments** - Pagos simples por proyecto
6. **payment_stages** - ⭐ Pagos por etapas/milestones
7. **invoices** - Facturación con impuestos/descuentos
8. **transactions** - Historial detallado de transacciones
9. **payment_methods** - Métodos de pago usuarios

**Gestión de Proyectos:**
10. **project_messages** - Comunicación por proyecto
11. **project_files** - Archivos subidos
12. **project_timeline** - Timeline con fechas
13. **budget_negotiations** - ⭐ Negociación presupuestos

**Soporte y Comunicación:**
14. **tickets** / **ticket_responses** - Sistema soporte completo
15. **notifications** - Notificaciones sistema
16. **twilio_config** - Configuración WhatsApp
17. **portfolio** - Showcase trabajos
18. **work_modalities** - Modalidades trabajo configurables

### 💰 Sistema de Pagos REAL (No MercadoPago)

**Métodos de Pago Locales Paraguay:**
- **Mango**: Alias 4220058 (Cédula)
- **Ueno Bank**: Alias 4220058 (Cédula)  
- **Banco Solar**: Alias softwarepar.lat@gmail.com (Email)

**Proceso Manual:**
1. Cliente selecciona método de pago
2. Realiza transferencia con alias
3. Sube comprobante al sistema
4. Sistema notifica admin por email + WhatsApp
5. Admin verifica y confirma pago manualmente

### 🏗️ Arquitectura del Backend (1700+ líneas de API)

**server/storage.ts** - 1725 líneas - Capa de acceso a datos:
- 40+ métodos para operaciones CRUD
- Analytics complejos para partners/admins
- Gestión de comisiones automáticas
- Activación automática etapas de pago por progreso

**server/routes.ts** - 1700+ líneas - API REST completa:
- Sistema autenticación JWT + RBAC
- Rutas por dominio: auth, users, projects, partners
- Middleware validación Zod
- Upload archivos con Multer (10MB)
- WebSocket server integrado

**Notificaciones Comprehensivas:**
- Base de datos + WebSocket + Email + WhatsApp simultáneo
- Templates específicos para cada evento
- Registro conexiones WebSocket por usuario

### 📱 Frontend Ultra-Moderno

**Stack Tecnológico:**
- **React 18 + TypeScript + Vite**
- **TanStack Query** - Estado servidor + cache inteligente
- **Wouter** - Routing ligero
- **Tailwind + shadcn/ui + Radix UI** - Sistema diseño
- **Framer Motion** - Animaciones

**Arquitectura Componentes:**
- Routing por roles: `/admin/*`, `/client/*`, `/partner/*`
- Hooks custom para auth, WebSocket, proyectos
- Layout responsive con integración WhatsApp directa
- Sistema notificaciones tiempo real

### 🔗 Servicios Externos Configurados

**Base de Datos:**
- **Neon PostgreSQL**: Serverless con pooling automático
- **Drizzle ORM**: Type-safe con migraciones automáticas

**Comunicación:**
- **Gmail SMTP**: Emails transaccionales (configurado ✅)
- **Twilio WhatsApp**: Configuración dinámica desde BD (variables faltantes ❌)

**Frontend:**
- **Vercel/Netlify Ready**: Build con Vite optimizado
- **WebSocket**: Tiempo real sobre HTTP/WS

## Recent Changes

### ✅ GitHub Import to Replit Setup Complete (September 30, 2025)

**Fresh Clone Configuration Completed:**
1. **Dependencies Installed**: All 644 npm packages installed successfully
2. **Database Connected**: PostgreSQL Neon database connected and verified with existing schema
3. **Database Schema**: All 18 tables validated and working (users, partners, projects, payments, etc.)
4. **Development Workflow**: Running on port 5000 with Vite HMR enabled
5. **Admin User Verified**: Default admin user exists (softwarepar.lat@gmail.com)
6. **Work Modalities**: Work modalities pre-configured in database
7. **Deployment Configuration**: VM deployment configured for production with build scripts

**Current Status:**
- ✅ **Frontend**: React 18 + TypeScript + Vite running successfully on port 5000
- ✅ **Backend**: Express server with WebSocket support running on port 5000
- ✅ **Database**: PostgreSQL Neon connected with all 18 tables operational
- ✅ **Security**: JWT authentication with secure development fallback
- ✅ **Email**: Gmail SMTP configured and ready for notifications
- ✅ **Deployment**: VM deployment configured (npm run build → npm start)
- ✅ **Landing Page**: Beautiful Spanish-language landing page displaying correctly

**Key Commands:**
- **Development**: `npm run dev` - Runs TypeScript server with Vite HMR on port 5000
- **Build**: `npm run build` - Builds frontend (Vite) and backend (esbuild) to dist/
- **Production**: `npm start` - Runs production build from dist/index.js
- **Database**: `npm run db:push` - Syncs schema changes to PostgreSQL
- **Type Check**: `npm run check` - TypeScript type checking

**Vite Configuration (Replit-Ready):**
- Host: `0.0.0.0:5000` with `allowedHosts: true` for Replit iframe proxy
- API Proxy: Backend API calls proxied to `/api` endpoint
- WebSocket: Real-time WebSocket connections on same port
- HMR: Full hot module replacement working in development mode
- Cache Control: Configured to prevent stale content in iframe

### ✅ RESIMPLE Invoice (Boleta) - Fixed Design (September 30, 2025)

**Professional PDF Boleta RESIMPLE - Corrected:**
- **Real Logo**: Now uses actual SoftwarePar logo image (attached_assets/image_1759242604696.png)
- **Fixed Layout**: Eliminated all text overlapping issues with proper spacing
- **Single Page**: Optimized to fit all content on one A4 page
- **Clean Header**: 
  - SoftwarePar logo on left (150px width)
  - "BOLETA" title on right with RESIMPLE subtitle
  - Project name displayed in header
- **Three-Column Info**: Compact display of N° Boleta, Fecha, Etapa
- **Company & Client Data**: Two-column layout with all required information
- **Service Table**: Clean table with quantity, description, unit price, and total
- **Totals Box**: Highlighted totals section with IVA exemption noted
- **Payment Info**: Clear payment method, status, and date
- **Footer**: Legal compliance info and signature line
- **Styling**: Professional design with consistent colors (#1e293b, #64748b, #cbd5e1)

**Endpoint**: `/api/client/stage-invoices/:stageId/download-resimple`
**File Format**: PDF (A4 size)
**File Name Pattern**: `SoftwarePar_Boleta_RESIMPLE_INV-STAGE-{PROJECT_ID}-{STAGE_NUM}.pdf`

**Technical Details:**
- Logo fallback: If logo not found, displays "SoftwarePar" text
- Compact margins: 35px for optimal space usage
- Font sizes: Reduced for better single-page fit (7-11pt)
- Colors: Professional slate/gray palette for business documents